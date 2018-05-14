<template>
  <div class="ksh-canvas-wrapper">
    <canvas ref="ksh-canvas"></canvas>
    <slot></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      provider: {
        //This is the CanvasRenderingContext that children will draw to.
        context: null,
        canvasWidth: 0,
        canvasHeight: 0,
        canvasInnerSpace: 48,
        canvasDivided: 5
      }
    }
  },
  //Allows any child component to 'inject:['provider'] and have access to it.
  provide () {
    return {
      provider : this.provider
    }
  },
  mounted () {
    // We  can't access  the rendering  context until the canvas is mounted to the DOM.
    // Once we have it,  provide it to all child components
    this.provider.context = this.$refs['ksh-canvas'].getContext('2d')
 
    // Resize the canvas to fit its parent's  width.
    // Normally you'd use a more flexible resize system
    this.provider.canvasWidth = this.$refs['ksh-canvas'].parentElement.clientWidth
    this.provider.canvasHeight = this.$refs['ksh-canvas'].parentElement.clientHeight

    this.$refs['ksh-canvas'].width  = this.provider.canvasWidth
    this.$refs['ksh-canvas'].height  = this.provider.canvasHeight
    
    if(!this.provider.context) return
    const ctx = this.provider.context
    
    ctx.beginPath()
    ctx.moveTo( this.provider.canvasInnerSpace,this.provider.canvasInnerSpace)
    ctx.lineTo( this.provider.canvasInnerSpace,this.provider.canvasHeight-this.provider.canvasInnerSpace)
    ctx.lineTo( this.provider.canvasWidth-this.provider.canvasInnerSpace, this.provider.canvasHeight-this.provider.canvasInnerSpace)
    ctx.strokeStyle = "#000000"
    ctx.stroke()
  }
}
</script>
<style scoped>
canvas {
  border:1px solid #000000;
  width: 80%;
  height: 80%;
}
</style>
