<template>
  <div class="container" v-on:mousemove="dragEvent" v-on:mouseup="clearDrag">
    <VoronoiColorDisplay
      :length="length"
      :width="width"
      :stretchHeight="stretchHeight"
      :lerpMode="lerpMode"
      :colorSteps="colorStepsComputed"
      v-on:click.native="setActive(-1)"
      v-on:size-notice="receiveSize"
    />
    <ColorNub
      v-for="(point, id) in colorSteps"
      :key="id"
      v-on:mousedown="setActive(id)"
      :selected="id == selected"
      :x="colorSteps[id].x"
      :y="colorSteps[id].y"
      :color="colorSteps[id].color"
    />
  </div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"
import VoronoiColorDisplay from "@/elements/VoronoiColorDisplay.vue"
import ColorNub from "@/elements/ColorNub.vue"
export default {
  name: "VoronoiColorXYSlider",
  status: "prototype",
  release: "0.0.2",
  components: {
    VoronoiColorDisplay,
    ColorNub,
  },
  props: {
    /**
     * selected nub. -1 = none
     */
    selectedNub: {
      type: Number,
      default: -1,
    },
    /**
     * Should the Y axis be ignored? (i.e. classic gradient tool mode)
     */
    oneDimensional: {
      type: Boolean,
      default: false,
    },
    /**
     * length in real world pixels
     */
    length: {
      type: Number,
      default: 100,
    },
    /**
     * relative width, length in real world pixels
     */
    width: {
      // rename?
      type: Number,
      default: 25,
    },
    /**
     * stretch height, if display aspect != real pixel aspect. overwrites computed aspect. give as height = stretch * width
     */
    stretchHeight: {
      type: Number,
    },
    /**
     * interpolation mode Possible: RGB, HSV
     */
    lerpMode: {
      type: String,
      default: "HSV",
      validator: value => {
        return value.match(/(RGB|HSV|nearest)/)
      },
    },
    /**
     * color points array as `[{x,y,color:{h,s,v}}]`
     */
    colorSteps: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      points: [
        {
          x: 0,
          y: 50,
          color: { h: 60, s: 0, v: 100 },
        },
        {
          x: 100,
          y: 0,
          color: { h: 120, s: 100, v: 100 },
        },
        {
          x: 100,
          y: 100,
          color: { h: 0, s: 100, v: 100 },
        },
      ],
      voronoiDimensions: { x: 0, y: 0 },
      dragging: false,
      selected: this.selectedNub,
    }
  },
  computed: {
    colorStepsComputed: function() {
      if (!this.oneDimensional) {
        return this.colorSteps
      }
      let colorStepsComputed = new Array(this.colorSteps.length)
      colorStepsComputed.fill({})
      for (let [ind, point] of this.colorSteps.entries()) {
        colorStepsComputed[ind] = {
          y: this.colorSteps[ind].y,
          x: 50,
          color: this.colorSteps[ind].color,
        }
      }
      return colorStepsComputed
    },
  },
  methods: {
    clearDrag() {
      this.dragging = false
    },
    dragEvent(e) {
      if (this.dragging) {
        e.preventDefault()
        if (e.path[0].className == "canvas") {
          this.colorSteps[this.selected].y = (e.offsetX / this.voronoiDimensions.width) * 100
          this.colorSteps[this.selected].x = (e.offsetY / this.voronoiDimensions.height) * 100
          /**
           * returns color points array for two way binding
           * @event color points dagged
           * @property {array} colorpoints
           */
          this.$emit("dragged", this.colorSteps)
        }
      }
    },
    receiveSize(e) {
      this.voronoiDimensions = e
    },
    setActive(id) {
      // helper to set active nub
      if (id >= 0) {
        this.dragging = true
      }
      /**
       * returns color points array for two way binding
       * @event nub selected
       * @property {number} ID
       */
      this.$emit("selected", id)
      this.selected = id
    },
  },
  watch: {
    selectedNub() {
      this.selected = this.selectedNub
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  transition: none !important;
  position: relative;
  box-shadow: $shadow-light;
}
.point {
  position: absolute;
  width: $size-fineprint;
  height: $size-fineprint;
  border-radius: $radius-force-circle;
  border: $space-xs solid var(--text-color);
  background-color: rgba(360, 100, 100, 0);
  &.selected {
    &::after {
      position: absolute;
      top: 25%;
      left: 25%;
      display: inline-block;
      content: " ";
      width: 50%;
      height: 50%;
      background-color: var(--text-color);
      border-radius: $radius-force-circle;
    }
  }
}
</style>

<docs>
  ### voronoi color display enhanced with interactive controls.
  ```vue
  <template lang="html">
    <div style="width: 200px">
      <VoronoiColorXYSlider
      :oneDimensional="true"
      lerpMode="RGB"
      :colorSteps="value"
      :selectedNub="selected"
      @dragged="e => value = e"
      @selected="e=> selected = e"
      />
      <hr/>
      <VoronoiColorXYSlider
      :oneDimensional="false"
      lerpMode="HSV"
      :colorSteps="value"
      :selectedNub="selected"
      @dragged="e => value = e"
      @selected="e=> selected = e"
      />
      <hr/>
      <VoronoiColorXYSlider
      :oneDimensional="false"
      lerpMode="RGB"
      :colorSteps="value"
      :selectedNub="selected"
      @dragged="e => value = e"
      @selected="e=> selected = e"
      />
    </div>
  </template>

  <script>
  export default {
    data(){
      return {
        selected: 1,
        value: [{
              x: 0,
              y: 50,
              color: {h: 60, s: 0, v: 100}
          },
          {
              x: 100,
              y: 0,
              color: {h: 120, s: 100, v: 100}
          },
          {
              x: 100,
              y: 100,
              color: {h: 0, s: 100, v: 100}
          }
        ]
      }

    }
  }
  </script>

  <style lang="css" scoped>
  </style>


  ```
</docs>
