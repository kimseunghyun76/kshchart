<script>
  // Note how there's no template or styles in this component.
  // Helper functions  to convert a percentage of canvas area to pixels.
  const percentWidthToPix = (percent, ctx) => Math.floor((ctx.canvas.width/100) * percent)
  const percentHeightToPix = (percent, ctx) => Math.floor((ctx.canvas.height/100) * percent)

  export default {
    // Gets us the provider property from the parent <ksh-canvas> component
    inject: ['provider'],
    props: {
      // The value to display
      value: {
        type: Number,
        defualt: 0
      }
    },
    data () {
      return {
        // We cache the dimensions of the previous
        // render so that we can clear the area later
        oldBox: {
          x: null,
          y: null,
          w: null,
          h: null
        }
      }
    },
    render() {
      // Since the parent canvas has to mount first, 
      // it's *possible* that the context may not be injected by the time this render funcion runs the first time.
      if(!this.provider.context) return
      const ctx = this.provider.context
      
    }
  }
</script>