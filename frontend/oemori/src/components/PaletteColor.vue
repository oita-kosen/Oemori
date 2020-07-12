<template>
  <transition name="highlight">
    <div :style="styles" @click="onClick">
      <slot class="palette-color-overlay"></slot>
    </div>
  </transition>
</template>

<script>
import tween from 'tween'

export default {
  data () {
    return {
      borderColor: '#818181',
      borderWidth: 2,
      size_: this.size,
      opacity: this.isCovered ? 0.3 : 1.0
    }
  },
  props: ['name', 'color', 'hasBorder', 'size', 'isCovered'],
  methods: {
    onClick (e) {
      this.$emit('select', this.name)
    }
  },
  computed: {
    styles () {
      let style = {
        margin: 'auto 0',
        backgroundColor: this.color,
        width: this.size_ + 'px',
        height: this.size_ + 'px',
        opacity: this.opacity,
        borderRadius: (this.size_ / 2.0 + this.borderWidth) + 'px'
      }
      if (this.hasBorder === true) {
        style.borderColor = this.borderColor
        style.borderWidth = this.borderWidth + 'px'
        style.borderStyle = 'solid'
      }
      return style
    }
  },
  watch: {
    size (newValue, oldValue) {
      console.log(this.name, 'size changed', oldValue, 'to', newValue)
      function animate () {
        if (tween.update()) {
          requestAnimationFrame(animate)
        }
      }
      let it = this
      // this.size = value
      let i = 0
      new tween.Tween({value: oldValue})
        .to({value: newValue}, 100)
        .easing(tween.Easing.Bounce.InOut)
        .onUpdate(function () {
          it.size_ = this.value.toFixed(0)
        })
        .start()
      animate()
    },
    isCovered (newValue, oldValue) {
      console.log(this.name, 'state changed', oldValue, 'to', newValue)
      function animate () {
        if (tween.update()) {
          requestAnimationFrame(animate)
        }
      }
      let it = this

      let from = 1.0
      let to = 0.3
      if (newValue === false) { // 非選択状態では無くなったら
        from = 0.3
        to = 1.0
      }
      // this.size = value
      let i = 0
      new tween.Tween({value: from})
        .to({value: to}, 400)
        .easing(tween.Easing.Bounce.InOut)
        .onUpdate(function () {
          // console.log('updating..', it.name, i++, this.value)
          it.opacity = this.value
        })
        .start()
      animate()
    }
  }
}
</script>

<style lang="stylus" scoped>
.palette-color-overlay{
  display: block;
}

.highlight-enter-active .highlight-leave-active {
  transition: opacity width height 0.5s;
}
</style>
