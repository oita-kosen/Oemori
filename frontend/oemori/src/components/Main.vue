<template>
  <div class="main-column" id="main_column">
    <Canvas class="canvas" 
      :lineColor="color"
      :lineWidth="width"
      :mode="mode"
      :canvasWidth="canvasWidth"
      :canvasHeight="canvasHeight"
    />
  </div>
</template>

<script>
import Canvas from './Canvas'
import Vue from 'vue'
export default {
  data() {
    return {
      color: "rgba(255, 0, 0, 1)",
      width: 3,
      mode: "pen",
      canvasWidth: 360,
      canvasHeight: 360,
      _canvasWrapper: null
    }
  },
  methods: {
  },
  computed: {
  },
  mounted() {
    const wrapper = document.getElementById("main_column")
    console.log(wrapper)
    this.canvasWidth = wrapper.clientWidth
    this.canvasHeight = wrapper.clientHeight
  }
}

Vue.component('Canvas', Canvas);

</script>

<style lang="stylus" scoped>
.main-column{

  witdh: 100%;
  height: 100%;
  z-index: 0;
}
</style>