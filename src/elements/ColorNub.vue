<template>
  <div
    :class="[selected ? 'selected' : null, 'point']"
    :style="pointStyle"
    v-on:mousedown="emitMouseDown"
  ></div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"
export default {
  name: "ColorNub",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * is this nub selected?
     */
    selected: {
      type: Boolean,
      default: false,
    },
    /**
     * x pos in percent relative to parent
     */
    x: {
      type: Number,
      default: 50,
    },
    /**
     * y pos in percent relative to parent
     */
    y: {
      type: Number,
      default: 50,
    },
    /**
     * color as hsv 360 100 100
     */
    color: {
      type: Object,
      default: () => {
        return {
          h: 255,
          s: 0,
          v: 100,
        }
      },
    },
  },
  computed: {
    borderStyle() {
      if (this.color.v < 50) {
        return designTokens.props.offwhite.value
      } else {
        return designTokens.props.offblack.value
      }
    },
    pointStyle() {
      let rgbCol = this.HSVToRGB(this.color)
      return {
        "--text-color": this.borderStyle,
        top: "calc(" + this.x + "% - calc( " + designTokens.props.size_paragraph.value + " / 2))",
        left: "calc(" + this.y + "% - calc( " + designTokens.props.size_paragraph.value + " / 2))",
        "background-color": "rgb(" + rgbCol.r + ", " + rgbCol.g + ", " + rgbCol.b + ")",
      }
    },
  },
  methods: {
    HSVToRGB(h, s, v) {
      var r, g, b, i, f, p, q, t
      if (arguments.length === 1) {
        ;(s = h.s), (v = h.v), (h = h.h)
      }
      h = h / 360
      s = s / 100
      v = v / 100
      i = Math.floor(h * 6)
      f = h * 6 - i
      p = v * (1 - s)
      q = v * (1 - f * s)
      t = v * (1 - (1 - f) * s)
      switch (i % 6) {
        case 0:
          ;(r = v), (g = t), (b = p)
          break
        case 1:
          ;(r = q), (g = v), (b = p)
          break
        case 2:
          ;(r = p), (g = v), (b = t)
          break
        case 3:
          ;(r = p), (g = q), (b = v)
          break
        case 4:
          ;(r = t), (g = p), (b = v)
          break
        case 5:
          ;(r = v), (g = p), (b = q)
          break
      }
      return {
        r: Math.round(r * 255),
        g: Math.round(g * 255),
        b: Math.round(b * 255),
      }
    },
    emitMouseDown(e) {
      /**
       * emit mousedown, pass default object
       * @event mousedown
       */
      this.$emit("mousedown", e)
    },
  },
}
</script>

<style lang="scss" scoped>
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
  ### Shows and selects a color in a color picker
  ```jsx
  <div class="">
    <div style="position: relative; height: 2rem">
      <ColorNub/>
    </div>
    <div style="position: relative; height: 2rem">
      <ColorNub
      :selected="true"
      :color="{h: 60, s: 100, v: 100}"
      />
    </div>
  </div>


  ```
</docs>
