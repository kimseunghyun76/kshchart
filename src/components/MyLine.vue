<script>
  // Note how there's no template or styles in this component.

  //Helper functions  to convert a percentage of canvas area to pixels.
  const percentWidthToPix = (percent, ctx) => Math.floor(((ctx.canvas.width - 100)/100) * percent)
  const percentHeightToPix = (percent, ctx) => Math.floor(((ctx.canvas.height - 50)/100) * percent)

  export default {
    // Gets us the provider property from the parent <ksh-canvas> component
    inject: ['provider'],
    props: {
      // Start coordinates (percentage of canvas dimensions)
      x1: {
        type: Number,
        default: 0
      },
      y1: {
        type: Number,
        default: 0
      },
      // End coordinates (percentage of canvas dimensions)
      x2: {
        type: Number,
        default: 0
      },
      y2: {
        type: Number,
        default: 0
      },
      // The value to display
      value: {
        type: Number,
        defualt: 0
      },
      // The color of the box.
      color: {
        type: String,
        default: '#F00'
      },
      // The name
      name: {
        type: String,
        defualt: 'SV'
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
    computed: {
      calculatedBox () {
        const ctx = this.provider.context
        //console.log(this.x1 + "," + percentWidthToPix(this.x1, ctx))
        //Turn start / end percentages into x,y , width, height in pixels
        const calculated = {
          x: (percentWidthToPix(this.x1, ctx) + 50) ,
          y: percentHeightToPix(this.y1, ctx),
          w: percentWidthToPix(this.x2 - this.x1, ctx),
          h: percentHeightToPix(this.y2 - this.y1 , ctx)       
        }

        // yes yes, side-effects.
        // This lets us cache the box dimensions of the previous render.
        // before we re-calculate calculatedBox the next render.
        this.oldBox = calculated
        return calculated
      }
    },
    render() {
      // Since the parent canvas has to mount first, 
      // it's *possible* that the context may not be injected by the time this render funcion runs the first time.
      if(!this.provider.context) return
      const ctx = this.provider.context
      
      // Keep a reference to the box used in the previous render call.
      const oldBox = this.oldBox
      // Calculate the new box. (Computed properties update on-demand.)
      const newBox = this.calculatedBox

      ctx.beginPath()
      ctx.arc(newBox.x + (newBox.w/2) , newBox.y+newBox.h, 5, 0, 2 * Math.PI, true)
      ctx.strokeStyle = "#ff0000"
      ctx.stroke()
      
      console.log(this.$parent.$children)
      //console.log(this.$parent.$children[this.$parent.$children.length - 1])
    }
  }
</script>