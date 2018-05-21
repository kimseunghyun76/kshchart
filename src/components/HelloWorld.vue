<template>
  <div class="hello">
    <!-- These are the custom components we'll create -->
    <!-- Values for `my-box` are percentages of the width of the canvas. -->
    <!-- Each bar will take up an equal space of the canvas. -->

    <div>
      <div>
        <h2> Mcc Gateway Message Queue Status </h2>
      </div>
      <div>
        <ksh-canvas>
          <!-- <my-box 
            v-for="(obj,index) in chartValues"
            :x1="((index / chartValues.length) * 100)"
            :x2="((index / chartValues.length) * 100) + (100 / chartValues.length)"
            :y1="100"
            :y2="100-obj.val"
            :color="obj.color"
            :value="obj.val"
            :name="obj.name"
            :idx="index"
            :barChart="true"
            :lineChart="false"
            :dotChart="true"
            v-bind:key="'b'+index"
          >
          </my-box> -->
        </ksh-canvas>
      </div>
    </div>
  </div>
</template>

<script>
import kshCanvas from './kshCanvas.vue'

export default {
  name: 'hello',
  components: {
    kshCanvas
  },
  data () {
    return {
      viewcount:20,
      chartValues: [
        {val: 124, name: '2017-04-01'},
        {val: 322, name: '2017-04-02'},
        {val: 536, name: '2017-04-03'},
        {val: 151, name: '2017-04-04'},
        {val: 268, name: '2017-04-05'},
        {val: 672, name: '2017-04-06'},
        {val: 204, name: '2017-04-07'},
        {val: 45, name: '2017-04-08'},
        {val: 4232, name: '2017-04-09'},
        {val: 1832, name: '2017-04-10'},
        {val: 224, name: '2017-04-11'},
        {val: 392, name: '2017-04-12'},
        {val: 576, name: '2017-04-13'},
        {val: 611, name: '2017-04-14'},
        {val: 298, name: '2017-04-15'},
        {val: 62, name: '2017-04-16'},
        {val: 224, name: '2017-04-17'},
        {val: 832, name: '2017-04-18'},
        {val: 382, name: '2017-04-19'},
        {val: 832, name: '2017-04-20'},
        {val: 274, name: '2017-04-21'},
        {val: 232, name: '2017-04-22'},
        {val: 656, name: '2017-04-23'},
        {val: 411, name: '2017-04-24'},
        {val: 28, name: '2017-04-25'},
        {val: 362, name: '2017-04-26'},
        {val: 24, name: '2017-04-27'},
        {val: 832, name: '2017-04-28'},
        {val: 1832, name: '2017-04-29'},
        {val: 332, name: '2017-04-30'},
      ]
    }
  },
  // Randomly selects a value to randomly increment or decrement every 16 ms.
  // Not really important, just demonstrates that reactivity still works.
  mounted: function (){
    // let dir = 1
    
    // let selectedVal = Math.floor(Math.random * this.chartValues.length)
    
    // //this.viewcount = 11
    // // setInterval(() => { 
    // //     this.viewcount = Math.min(Math.max(Math.floor(Math.random() * this.chartValues.length), 5), this.chartValues.length-1) 
    // // }, 10000)

    // setInterval(() => {
    //   if (Math.random() > 0.695) dir *= -1;
    //   if (Math.random() > 0.69) selectedVal = Math.floor(Math.random() * this.chartValues.length)
    //   if(!isNaN(selectedVal)){
    //     this.chartValues[selectedVal].val = Math.min(Math.max(this.chartValues[selectedVal].val + dir * 0.5, 5), 95)  
    //   }
    // }, 1)
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
  height: 100%;
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
}

input {
  height: 40px;
  font-size : 20px;
  font-family : 'NanumGothic';
}
</style>
