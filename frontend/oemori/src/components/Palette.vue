<template>
  <div class="palette">
    <div class="palette-brush-container">
      <palette-color class="palette-brush"
          name=eraser
          :color="colors.white.color"
          :hasBorder="true"
          :size="paletteColorSizeSmall+10"
          @select="onSelectBrush">
        <i class="palette-icon fas fa-eraser"></i>
      </palette-color>
    </div>
    <div class="palette-color-container">
      <div class="palette-color">
        <palette-color v-for="color in colors" :key="color.id"
          :name="color.name"
          :color="color.color"
          :hasBorder="color.hasBorder"
          :size="color.name == selectedColorName ? paletteColorSizeBig : paletteColorSizeSmall"
          :isCovered="color.name != selectedColorName"
          @select="onSelectColor"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import PaletteColor from './PaletteColor'

export default {
  data () {
    return {
      paletteColorSizeSmall: 42,
      paletteColorSizeBig: 45
    }
  },
  props: ['colors', 'selectedColorName'],
  methods: {
    onSelectColor (colorName) {
      console.log('color name:', colorName)
      // this.selectedColorName = colorName
      this.$emit('colorSelected', this.colors[colorName])
    },
    onSelectBrush (brushName) {
      console.log('brush name:', brushName)
      // if (brushName === 'eraser') {
      //   // this.selectedColorName = null
      // }
      this.$emit('brushSelected', brushName)
    }
  }
}

Vue.component('palette-color', PaletteColor)
</script>

<style lang="stylus" scoped>
.palette {
  display: flex;
  flex-direction: row;
  width: 600px;
  height: 80px;
  box-sizing: border-box;
  padding: 0 20px;
  border-radius: 33px;
  box-shadow: rgba(0, 0, 0, 0.16) 1px 3px 6px;
  background-color: #FFFDBA;

  &-icon {
    font-size: 30px;
    vertical-align: middle;
    height: 30px;
    margin: auto;
  }

  &-brush-container {
    margin: auto 0;
    width: 15%;
  }

  &-brush {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
  }

  &-color-container {
    margin: auto 0;
    width: 85%;
  }

  &-color {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
  }
}
</style>
