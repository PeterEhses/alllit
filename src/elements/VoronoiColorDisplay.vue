<template>
  <div class="container" :style="{ height: containerHeight }">
    <canvas class="canvas" ref="canvas" v-on:click="drawrect()"></canvas>
  </div>
</template>

<script>
export default {
  name: "VoronoiColorDisplay",
  status: "prototype",
  release: "0.0.1",
  components: {},
  props: {
    /**
     * dummy
     */
    /**
     * The fill color of the SVG icon.
     */
    dummy: {
      type: String,
      default: "dummy",
    },
  },
  data() {
    return {
      rectWidth: 100,
      dimensions: {
        width: 0,
        height: 0,
      },
      observer: null,
    }
  },
  computed: {
    canvas: function() {
      return this.$refs.canvas.getContext("2d")
    },
    containerHeight: function() {
      return this.dimensions.width * 0.5
    },
  },
  watch: {
    dimensions: {
      handler() {
        console.log(this.dimensions)
        this.canvas.width = this.dimensions.width
        this.canvas.height = this.dimensions.height
      },
      deep: true,
    },
  },
  methods: {
    callbackObserver(mutationsList) {
      for (const mutation of mutationsList) {
        if (mutation.contentRect) {
          this.dimensions.width = this.$el.clientWidth
          this.dimensions.height = this.$el.clientHeight
        }
      }
    },
    drawrect() {
      // clear canvas
      this.canvas.clearRect(0, 0, 400, 200)

      // draw rect
      this.canvas.beginPath()
      this.canvas.rect(20, 20, this.rectWidth, 100)
      this.canvas.stroke()
    },
    addWidth() {
      this.rectWidth += 10
      this.drawRect()
    },
    subWidth() {
      this.rectWidth -= 10
      this.drawRect()
    },
  },
  mounted() {
    this.dimensions.width = this.$el.clientWidth
    this.dimensions.height = this.$el.clientHeight
    let targetNode = this.$el
    let config = { attributes: true }
    this.observer = new ResizeObserver(this.callbackObserver)
    this.observer.observe(targetNode, config)
  },
  destroyed() {
    this.observer.disconnect()
  },
}
</script>

<style lang="scss" scoped>
.container,
.canvas {
  @include reset;
}
</style>

<docs>
  ```jsx
  <VoronoiColorDisplay/>
  ```
</docs>
