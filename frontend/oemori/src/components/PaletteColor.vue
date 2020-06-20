<template>
  <div :style="styles" @click="onClick">
    <slot class="palette-color-overlay"></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      borderColor: "#818181",
      borderWidth: 2
    }
  },
  props: ['name', 'color', 'hasBorder', 'size'],
  methods: {
    onClick(e){
      this.$emit('select', this.name);
    }
  },
  computed: {
    styles(){
      let style = {
        margin: "auto 0",
        backgroundColor: this.color,
        width: this.size+"px",
        height: this.size+"px",
        borderRadius: (this.size/2.0+this.borderWidth)+"px"
      }
      if(this.hasBorder == true){
        style.borderColor=this.borderColor;
        style.borderWidth=this.borderWidth+"px";
        style.borderStyle="solid";
      }
      return style
    }
  }
}
</script>

<style lang="stylus" scoped>
.palette-color-overlay{
  display: block;

}
</style>