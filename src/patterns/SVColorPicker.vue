<template>
  <div
    class="base"
    :style="{ background: gradient }"
    v-on:mousemove="dragEvent"
    v-on:mouseup="dragActive = false"
    v-on:mousedown="dragActivate"
  >
    <ColorNub
      v-on:mousedown="dragActivate"
      :color="internalColor"
      :x="100 - internalColor.v"
      :y="internalColor.s"
    />
  </div>
</template>

<script>
import ColorNub from "@/elements/ColorNub.vue"
export default {
  name: "SVColorPicker",
  status: "prototype",
  release: "0.0.1",
  components: {
    ColorNub,
  },
  props: {
    /**
     * color object
     */
    color: {
      type: Object,
      default: () => {
        return {
          h: 200,
          s: 50,
          v: 100,
        }
      },
    },
  },
  data() {
    return {
      internalColor: this.color,
      dragActive: false,
    }
  },
  computed: {
    gradient() {
      let grad =
        "linear-gradient(0deg, hsla(0, 0%, 0%, 1), hsla(0, 0%, 0%, 0)), \n \
      linear-gradient(-90deg, hsla(" +
        this.color.h +
        ", 100%, 50%, 1), hsla(0, 0%, 100%, 1))"
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
        if (e.path[0].className == "base") {
          this.internalColor.s = (e.offsetX / this.$el.clientWidth) * 100
          this.internalColor.v = 100 - (e.offsetY / this.$el.clientHeight) * 100
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
.base {
  box-shadow: $shadow-light;
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: $size-fineprint/2 + $space-xs * 2; // because the nub has to fit perfectly
}
</style>

<docs>
  ```jsx
  <div style="width: 250px; height: 250px;">
    <SVColorPicker/>
  </div>

  ```
</docs>
