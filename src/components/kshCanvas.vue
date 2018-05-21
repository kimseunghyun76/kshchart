<template>
  <div class="ksh-canvas-wrapper">
    <canvas 
      ref="ksh-canvas"
      v-on:mouseup="handleMouseUp"
      v-on:mousedown="handleMouseDown"
      v-on:mousemove="handleMouseMove"
      v-on:mousewheel="doScroll"
    ></canvas>
    <slot></slot>
  </div>
</template>
<script>
export default {
  data() {
    return { 
      boxes:{
        max:0,
        count:0,
        viewcnt: 10,
        line:{
          strokeStyle:'#888888',
          fillStyle:'rgba(190, 20, 0, 0.5)'
        },
        rect:{
          strokeStyle:'#888888',
          fillStyle:'rgba(190, 20, 0, 0.5)'
        },
        selected:{
          index:-1,
          x:0,
          y:0,
          rect:{
            strokeStyle:'#888888',
            fillStyle:'rgba(190, 20, 0, 0.5)'
          },
          line:{
            strokeStyle:'#888888',
            fillStyle:'rgba(190, 20, 0, 0.5)'
          }
        },
        val:[]
      },
      canvas:{
        spanx:5,
        spany:10,
        context: null,
        w: 0,
        h: 0,
        sx: 0,
        sy: 0,
        sw: 0,
        sh: 0
      },
      mouse: {
        down: false,
        current:{
          x: 0,
          y: 0
        },
        previous: {
          x: 0,
          y: 0
        }
      }
    }
  },
  mounted () {
    this.canvas.context = this.$refs['ksh-canvas'].getContext('2d')
    this.canvas.w = this.$refs['ksh-canvas'].parentElement.clientWidth
    this.canvas.h = this.$refs['ksh-canvas'].parentElement.clientHeight
    this.$refs['ksh-canvas'].width  = this.canvas.w
    this.$refs['ksh-canvas'].height  = this.canvas.h
    
    this.boxes.count = this.$parent.chartValues.length
    
    //x축과 y축에 값을 넣기 위해서 여백 비율(%)을 줘야 함.
    this.canvas.sx = this.canvas.w * (this.canvas.spanx / 100)
    this.canvas.sy = this.canvas.h * (this.canvas.spany / 100)
    this.canvas.sw = this.canvas.w - (this.canvas.sx * 2)
    this.canvas.sh = this.canvas.h - (this.canvas.sy * 2)

    this.draw()
  },
  methods : {
    handleMouseUp : function(evt){
      this.mouse.down = false
      
      this.boxes.val.forEach((el,index) =>{
        el.w = this.canvas.sw /this.boxes.viewcnt
        el.x = (index*el.w) + this.canvas.sx
        if(el.x < evt.pageX && evt.pageX < (el.x + el.w) ){
          this.boxes.selected.index = index
          
          /** Clear Canvas and Draw X&Y Axis  */
          this.clearchart()
          /** Draw Bar Graph */
          this.drawBarGraph()
          return  
        }
      })
      
      evt.preventDefault()
    },
    handleMouseDown : function(evt){
      this.mouse.down = true
      this.mouse.current = {
        x: evt.pageX,
        y: evt.pageY
      }
      evt.preventDefault()
    },
    handleMouseMove : function(evt){
      if(this.mouse.down){
        this.mouse.previous = {
          x: evt.pageX,
          y: evt.pageY
        }
        this.zoom(this.mouse.current.x , this.mouse.previous.x)
      }
      evt.preventDefault()
    },
    doScroll: function(evt){
      var delta = Math.max(-1, Math.min(1, (evt.wheelDelta || -evt.detail)))
      this.zoom(delta , 0)
      evt.preventDefault()
    },
    zoom:function(x1,x2){
      if( x1 > x2 ){
        if(this.boxes.viewcnt > 1){
          this.boxes.viewcnt--
          this.draw()
        }
      }else{
        if(this.boxes.viewcnt < this.boxes.count){
          this.boxes.viewcnt++
          this.draw()
        }
      }
    },
    drawline: function(strokestyle, linewidth, x1, y1, x2, y2){
      if(!this.canvas.context) return
      const ctx = this.canvas.context
      ctx.beginPath()
      ctx.strokeStyle = strokestyle
      ctx.lineWidth = linewidth
      ctx.moveTo( x1, y1)
      ctx.lineTo( x2, y2)
      ctx.stroke()
    },
    drawrect: function(strokestyle, linewidth, fillStyle, x, y, w, h){
      if(!this.canvas.context) return
      const ctx = this.canvas.context
      ctx.beginPath()
      ctx.strokeStyle = strokestyle
      ctx.lineWidth = linewidth
      ctx.strokeRect(x, y, w, h)
      ctx.rect(x, y, w, h)
      ctx.fillStyle = fillStyle
      ctx.fill()
    },
    clearcanvas: function(){
      if(!this.canvas.context) return
      const ctx = this.canvas.context
      
      ctx.clearRect(0,0,this.canvas.w,this.canvas.h)
      /** X & Y axis */
      /** X axis */
      this.drawline(
        "#000000",
        2,
        this.canvas.sw + this.canvas.sx,
        0 + this.canvas.sy,
        this.canvas.sw + this.canvas.sx,
        this.canvas.sh + this.canvas.sy
      )
      /** Y axis */
      this.drawline(
        "#000000",
        2,
        this.canvas.sw + this.canvas.sx,
        this.canvas.sh + this.canvas.sy,
        this.canvas.sx,
        this.canvas.sh + this.canvas.sy
      )
    },
    clearchart: function(){
      if(!this.canvas.context) return
      const ctx = this.canvas.context
      ctx.clearRect(this.canvas.sx,this.canvas.sy,this.canvas.sw,this.canvas.sh)
    },
    initBoxes: function(){
      /** Set Graph Data for Canvas */
      this.boxes.val.length = 0
      this.boxes.max = 0
      this.$parent.chartValues.forEach((el,index) => {
        if(this.boxes.count - index <= this.boxes.viewcnt){
          if(this.boxes.max < el.val){
            this.boxes.max = el.val
          }
          this.boxes.val.push(  { v:el.val, name:el.name, x : null , y : null , w: null , h: null })
        }
      })
    },
    drawBarGraph: function () {
      if(!this.canvas.context) return
      const ctx = this.canvas.context
      /** Draw Bar Graph*/
      this.boxes.val.forEach((el,index) =>{
        el.w = this.canvas.sw /this.boxes.viewcnt
        el.h = - (this.canvas.sh* (el.v/this.boxes.max))
        el.x = (index*el.w) + this.canvas.sx
        el.y = this.canvas.sh + this.canvas.sy
        if(this.boxes.selected.index === index){
          this.drawrect(
            "#FF0000", 
            1, 
            'rgba(190, 20, 0, 0.5)',
            el.x, 
            el.y, 
            el.w, 
            el.h
          )
          //Selected Guide line
          this.drawline(
            '#FF0000',
            2,
            el.x + el.w, 
            el.y  + el.h,
            el.x + el.w + this.canvas.sw - (el.w *(index+1)),
            el.y  + el.h 
          )
        }else{  
          this.drawrect(
            "#888888", 
            1, 
            'rgba(90, 120, 10, 0.5)',
            el.x, 
            el.y, 
            el.w, 
            el.h
          )
          this.drawline(
            '#FFFFFF',
            2,
            el.x + el.w, 
            el.y + el.h,
            el.x + el.w + this.canvas.sw - (el.w *(index+1)),
            el.y  + el.h 
          )
        }
      })
    },
    draw: function () {
      if(!this.canvas.context) return
      const ctx = this.canvas.context

      /** Clear Canvas and Draw X&Y Axis  */
      this.clearcanvas()
      /** Init Graph Data  */
      this.initBoxes()
      /** Draw Bar Graph */
      this.drawBarGraph()
      /** 수치 입력 */
      ctx.beginPath()





      ctx.fillStyle = '#000'
      ctx.textAlign = 'center'
      this.boxes.val.forEach((el,index) =>{
        el.w = this.canvas.sw /this.boxes.viewcnt
        el.h = - (this.canvas.sh* (el.v/this.boxes.max))
        el.x = (index*el.w) + this.canvas.sx
        el.y = this.canvas.sh + this.canvas.sy
        if( index % (Math.floor(this.boxes.viewcnt/10) + 1) === 0){
          if(el.name.length <= 10){
            ctx.font = '14px NanumGothic'
            ctx.fillText(el.name,el.x + (el.w/2), el.y + (this.canvas.sy/2) -10 )
          }else{
            ctx.font = '14px NanumGothic'
            ctx.fillText(el.name.substring(0,10)+'...',el.x + (el.w/2), el.y + (this.canvas.sy/2) -10 )
          }
        }
      })
      for(var i = 1 ; i <= 6 ; i++){
        //Y Axis amount
        ctx.fillStyle = '#000'
        ctx.font = '14px NanumGothic'
        ctx.textAlign = 'left'
        ctx.fillText((this.boxes.max *(i/6)).toFixed(2) ,(this.canvas.w-this.canvas.sx) +5, this.canvas.sh*((7-i)/6)-10)
        //Y Axis Guide line
        this.drawline(
          '#bbbbbb',
          1,
          this.canvas.sx,
          this.canvas.sh*((7-i)/6)-20,
          this.canvas.w-this.canvas.sx,
          this.canvas.sh*((7-i)/6)-20 
        )
      }
    }
  }
}

</script>
<style scoped>
canvas {
  border:1px solid #000000;
  width: 100%;
  height: 100%;
}
</style>
