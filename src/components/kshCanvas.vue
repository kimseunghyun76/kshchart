<template>
  <div class="ksh-canvas-wrapper">
    <canvas ref="ksh-canvas"></canvas>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    // Start coordinates (percentage of canvas dimensions)
    viewcount: {
      type: Number,
      default: 10
    }
  },
  data() {
    return { 
      boxesStat:{
        max:0,
        count:0
      },
      canvas:{
        context: null,
        w: 0,
        h: 0,
        sx: 0,
        sy: 0,
        sw: 0,
        sh: 0
      }
    }
  },
  watch: {
    viewcount: function (val){
      if(!this.canvas.context) return
      const ctx = this.canvas.context

      /** X & Y Axis */
      //x축과 y축에 값을 넣기 위해서 여백 비율(%)을 줘야 함.
      const spanx = 5
      const spany = 10
      this.canvas.sx = this.canvas.w * (spanx / 100)
      this.canvas.sy = this.canvas.h * (spany / 100)
      this.canvas.sw = this.canvas.w - (this.canvas.sx * 2)
      this.canvas.sh = this.canvas.h - (this.canvas.sy * 2)

      let boxes = []
      /** X & Y Axis */
      this.boxesStat.count = this.$parent.chartValues.length
      this.boxesStat.max = 0
      this.$parent.chartValues.forEach((el,index) => {
        if(this.boxesStat.count - index <= this.viewcount){
          if(this.boxesStat.max < el.val){
            this.boxesStat.max = el.val
          }
          boxes.push(  { v:el.val, name:el.name, x : null , y : null , w: null , h: null })
        }
      })
      
      /** Clear Canvas  */
      ctx.clearRect(0,0,this.canvas.w,this.canvas.h)

      /** Draw Bar */
      boxes.forEach((el,index) =>{
        el.w = this.canvas.sw /this.viewcount
        el.h = - (this.canvas.sh* (el.v/this.boxesStat.max))
        el.x = (index*el.w) + this.canvas.sx
        el.y = this.canvas.sh + this.canvas.sy
        ctx.beginPath()
        ctx.strokeStyle="#888888"
        ctx.strokeRect(el.x, el.y, el.w, el.h)
        ctx.rect(el.x, el.y, el.w, el.h)
        ctx.fillStyle = 'rgba(90, 120, 10, 0.5)'
        ctx.fill()
      })

      ctx.beginPath()
      ctx.strokeStyle = "#000000"
      ctx.moveTo( this.canvas.sx,this.canvas.sy)
      ctx.lineTo( this.canvas.sx,this.canvas.h-this.canvas.sy)
      ctx.lineTo( this.canvas.w-this.canvas.sx, this.canvas.h-this.canvas.sy)
      ctx.stroke()
      
      // /** X & Y Axis Background */
      // ctx.strokeStyle = "#eeeeee"
      // var bglinecnt = 20
      // ctx.beginPath()
      // for(var i = 0 ; i < bglinecnt ; i++){      
      //   ctx.moveTo( this.canvas.sx+ (this.canvas.sw*i/bglinecnt),this.canvas.sy)
      //   ctx.lineTo( this.canvas.sx+ (this.canvas.sw*i/bglinecnt),this.canvas.h-this.canvas.sy)
      // }
      //ctx.stroke()

      /** 수치 입력 */
      ctx.beginPath()
      ctx.fillStyle = '#000'
      ctx.textAlign = 'center'
      boxes.forEach((el,index) =>{
        el.w = this.canvas.sw /this.viewcount
        el.h = - (this.canvas.sh* (el.v/this.boxesStat.max))
        el.x = (index*el.w) + this.canvas.sx
        el.y = this.canvas.sh + this.canvas.sy
        if( index % (Math.floor(this.viewcount/10) + 1) === 0){
          if(el.name.length <= 10){
            ctx.font = '14px NanumGothic'
            ctx.fillText(el.name,el.x + (el.w/2), el.y + (this.canvas.sy/2) -10 )
          }else{
            ctx.font = '12px NanumGothic'
            ctx.fillText(el.name.substring(0,10)+'...',el.x + (el.w/2), el.y + (this.canvas.sy/2) -10 )
          }
        }
      })
      for(var i = 1 ; i <= 6 ; i++){
        ctx.fillStyle = '#000'
        ctx.font = '12px NanumGothic'
        ctx.textAlign = 'right'
        ctx.fillText((this.boxesStat.max *(i/6)).toFixed(2) ,this.canvas.sx -5, this.canvas.sh*((7-i)/6)-10)
        
        ctx.beginPath()
        ctx.strokeStyle = "#bbbbbb"
        ctx.moveTo( this.canvas.sx,this.canvas.sh*((7-i)/6)-20)
        ctx.lineTo( this.canvas.w-this.canvas.sx,this.canvas.sh*((7-i)/6)-20)
        ctx.stroke()
      }
    }
  },
  //Allows any child component to 'inject:['provider'] and have access to it.
  provide () {
    return {
      canvas : this.canvas
    }
  },
  mounted () {

    // We  can't access  the rendering  context until the canvas is mounted to the DOM.
    // Once we have it,  provide it to all child components
    this.canvas.context = this.$refs['ksh-canvas'].getContext('2d')
    // // Resize the canvas to fit its parent's  width.
    // // Normally you'd use a more flexible resize system
    this.canvas.w = this.$refs['ksh-canvas'].parentElement.clientWidth
    this.canvas.h = this.$refs['ksh-canvas'].parentElement.clientHeight

    this.$refs['ksh-canvas'].width  = this.canvas.w
    this.$refs['ksh-canvas'].height  = this.canvas.h

  }
}

var doScroll = function (e) {
    // cross-browser wheel delta
    e = window.event || e;
    var delta = Math.max(-1, Math.min(1, (e.wheelDelta || -e.detail)))
    // Do something with `delta`
    console.log(delta)
    e.preventDefault();
};

if (window.addEventListener) {
    window.addEventListener("mousewheel", doScroll, false);
    window.addEventListener("DOMMouseScroll", doScroll, false);
} else {
    window.attachEvent("onmousewheel", doScroll);
}
</script>
<style scoped>
canvas {
  border:1px solid #000000;
  width: 100%;
  height: 100%;
}
</style>
