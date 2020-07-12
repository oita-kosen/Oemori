<template>
  <div class="main-column" id="main_column">
    <Canvas class="canvas" 
      :lineColor="color"
      :lineWidth="width"
      :mode="mode"
      :canvasWidth="canvasWidth"
      :canvasHeight="canvasHeight"
    />
    <palette class="palette"
      :colors="colors"
      @colorSelected="onColorSelected"
      @brushSelected="onBrushSelected"
    />
  </div>
</template>

<script>

import Canvas from './Canvas'
import Palette from './Palette'
import Vue from 'vue'
export default {
  data() {
    return {
      color: "rgba(255, 0, 0, 1)",
      colors:  {
        white: { name:"white", color: "#FEFEFE", hasBorder: true },
        red: { name:"red", color: "#FF2762", hasBorder: false },
        purple: { name:"purple", color: "#D462DC", hasBorder: false },
        blue: { name:"blue", color: "#2F56F3", hasBorder: false },
        skyblue: { name:"skyblue", color: "#3BA4FF", hasBorder: false },
        green: { name:"green", color: "#9AF356", hasBorder: false },
        yellow: { name:"yellow", color: "#F2F92C", hasBorder: true }, 
        orange: { name:"orange", color: "#FFAD33", hasBorder: false }, 
        black: { name:"black", color: "#333333", hasBorder: false },
      },
      width: 10,
      mode: "pen",
      canvasWidth: 360,
      canvasHeight: 360,
      eraserWidth: 30, // TODO: Canvasに渡す消しゴム用の太さパラメータを作るべき？
      penWidth: 10
    }
  },
  methods: {
    onColorSelected(color){
      console.log('selected color:', color.color);
      this.color = color.color;
      // 消しゴムモードの時は強制的にペンモードにする
      if(this.mode === "eraser"){
        this.mode = "pen";
        this.width = this.penWidth;
        // this.width = 30;
      }
    },
    onBrushSelected(brushName){
      this.mode = brushName;
      this.width = this.eraserWidth;
      console.log("eraser width:", this.eraserWidth);
    }
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

Vue.component('Canvas', Canvas)
Vue.component('palette', Palette)

</script>

<style lang="stylus" scoped>
.main-column{

  witdh: 100%;
  height: 100%;
  z-index: 0;
}

.palette{
  position:fixed;
  bottom: 40px;
  left: 350px;
  z-index: 10;
}
</style>