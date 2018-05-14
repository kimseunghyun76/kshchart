<template>
  <div class="hello">
    <h2> Mcc Gateway Message Queue Status </h2>
    <!-- These are the custom components we'll create -->
    <!-- Values for `my-box` are percentages of the width of the canvas. -->
    <!-- Each bar will take up an equal space of the canvas. -->
    <ksh-canvas>
      <my-box 
        v-for="(obj,index) in chartValues"
        :x1="((index / chartValues.length) * 100)"
        :x2="((index / chartValues.length) * 100) + (100 / chartValues.length)"
        :y1="100"
        :y2="100-obj.val"
        :color="obj.color"
        :value="obj.val"
        :name="obj.name"
        v-bind:key="index"
      >
      </my-box>
      <my-axis
        v-for="(obj,index) in chartValues"
        :value="obj.val"
        v-bind:key="'a'+index"
      ></my-axis>
    </ksh-canvas>
  </div>
</template>

<script>
import kshCanvas from './kshCanvas.vue'
import MyAxis from './MyAxis.vue'
import MyBox from './MyBox.vue'

export default {
  name: 'hello',
  components: {
    kshCanvas,
    MyAxis,
    MyBox
  },
  data () {
    return {
      chartValues: [
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'},
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'},
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'},
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'},
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'},
        {val: 24, name: 'sv10', color: 'rgba(90, 120, 10, 0.5)'},
        {val: 32, name: 'sv20', color: 'rgba(100, 110, 250, 0.5)'},
        {val: 66, name: 'sv30', color: 'rgba(10, 140, 130, 0.5)'},
        {val: 11, name: 'sv40', color: 'rgba(100, 180, 30, 0.5)'},
        {val: 28, name: 'sv51', color: 'rgba(190, 120, 230, 0.5)'},
        {val: 62, name: 'sv52', color: 'rgba(240, 220, 130, 0.5)'}
      ]
    }
  },
  // Randomly selects a value to randomly increment or decrement every 16 ms.
  // Not really important, just demonstrates that reactivity still works.
  mounted: function (){
    let dir = 1
    let selectedVal = Math.floor(Math.random * this.chartValues.length)
    setInterval(() => {
      if (Math.random() > 0.395) dir *= -1;
      if (Math.random() > 0.29) selectedVal = Math.floor(Math.random() * this.chartValues.length)
      if(!isNaN(selectedVal)){
        this.chartValues[selectedVal].val = Math.min(Math.max(this.chartValues[selectedVal].val + dir * 10.1, 5), 95)
      }
    }, 1)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  position: relative;
  height: 100vh;
  width: 100vw;
  padding: 20px;
  box-sizing: border-box;
}
</style>
