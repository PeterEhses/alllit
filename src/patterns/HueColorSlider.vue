<template>
  <div
    class="slido"
    :style="{ background: gradient }"
    v-on:mousemove="dragEvent"
    v-on:mouseup="dragActive = false"
    v-on:mousedown="dragActivate"
  >
    <ColorNub :y="51" :x="color.h / 3.6" :color="displayColor" v-on:mousedown="dragActivate" />
  </div>
</template>

<script>
import ColorNub from "@/elements/ColorNub.vue"
export default {
  name: "HueColorPicker",
  status: "prototype",
  release: "0.0.1",
  components: {
    ColorNub,
  },
  props: {
    /**
     * color to set Hue component of
     */
    color: {
      type: Object,
      default: () => {
        return {
          h: 120,
          s: 100,
          v: 100,
        }
      },
    },
    /**
     * rainbow display steps granularity
     */
    granularity: {
      type: Number,
      default: 24,
    },
  },
  data() {
    return {
      dragActive: false,
      internalColor: this.color,
    }
  },
  computed: {
    displayColor() {
      return {
        h: this.internalColor.h,
        s: 100,
        v: 100,
      }
    },
    gradient: function() {
      let grad = "linear-gradient( 0deg, "
      let granularity = this.granularity
      for (let i = 0; i < granularity; i++) {
        grad += "hsl(" + (360 - i * (360 / granularity)) + ", 100%, 50%) "
        if (i < granularity - 1) {
          grad += ","
        }
      }
      grad += ")"
      return grad
    },
  },
  watch: {
    color() {
      this.internalColor = this.color
    },
  },
  methods: {
    dragActivate(e) {
      this.dragActive = true
      this.dragEvent(e)
    },
    dragEvent(e) {
      if (this.dragActive) {
        e.preventDefault()
        if (e.path[0].className == "slido") {
          this.internalColor.h = (e.offsetY / this.$el.clientHeight) * 360
          /**
           * returns color points array for two way binding
           * @event color point dagged
           * @property {object} color
           */
          this.$emit("dragged", this.internalColor)
        }
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.slido {
  box-shadow: $shadow-light;
  position: relative;
  width: $size-fineprint + $space-xs * 2;
  height: 100%;
  border-radius: $radius-force-circle;
  background-color: red;
}
</style>

<docs>
  ```jsx
  <div style="height: 200px;">
    <HueColorPicker/>
  </div>
  ```
</docs>
