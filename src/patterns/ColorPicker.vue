<template>
  <div class="colorpicker">
    <div class="topcontrols">
      <span>
        <Icon name="add" fill="var(--text-color)" size="inline" v-on:click.native="addColor" />
        <Icon
          name="minimize"
          fill="var(--text-color)"
          size="inline"
          v-on:click.native="removeColor"
        />
      </span>
      <TwoStateSwitch left="1D" right="2D" v-model="oneDimensional" />
      <TwoStateSwitch left="nearest" right="gradient" v-model="lerpModeBool" />
    </div>
    <VoronoiColorXYSlider
      :length="144"
      :width="1"
      :stretchHeight="0.2"
      :oneDimensional="!oneDimensional"
      :lerpMode="lerpMode"
      :colorSteps="colorNubs"
      @dragged="e => (colorNubs = e)"
      :selectedNub="selectedColorNub"
      @selected="e => (selectedColorNub = e)"
    />
  </div>
</template>

<script>
import Icon from "@/elements/Icon.vue"
import TwoStateSwitch from "@/patterns/TwoStateSwitch.vue"
import VoronoiColorXYSlider from "@/patterns/VoronoiColorXYSlider.vue"
export default {
  name: "ColorPicker",
  status: "prototype",
  release: "0.0.1",
  components: {
    Icon,
    TwoStateSwitch,
    VoronoiColorXYSlider,
  },
  props: {
    /**
     * dummy
     */
    dummy: {
      type: String,
      default: "dummy",
    },
  },
  data() {
    return {
      selectedColorNub: 0,
      oneDimensional: false,
      lerpModeBool: false,
      colorNubs: [
        // {
        //   x: 50,
        //   y: 50,
        //   color: {
        //     h: 0,
        //     s: 100,
        //     v: 100
        //   }
        // }
      ],
    }
  },
  computed: {
    lerpMode() {
      if (this.lerpModeBool) {
        return "RGB"
      } else {
        return "nearest"
      }
    },
  },
  methods: {
    accentHue() {
      return getComputedStyle(this.$el).getPropertyValue("--primary-hue")
    },
    hsl2hsv(hue, sat, light) {
      sat *= light < 0.5 ? light : 1 - light

      return {
        //[hue, saturation, value]
        //Range should be between 0 - 1

        h: hue, //Hue stays the same
        s: (2 * sat) / (light + sat), //Saturation
        v: light + sat, //Value
      }
    },
    removeColor() {
      if (
        this.selectedColorNub >= 0 &&
        this.selectedColorNub < this.colorNubs.length &&
        this.colorNubs.length > 1
      ) {
        this.colorNubs.splice(this.selectedColorNub, 1)
        if (this.selectedColorNub >= this.colorNubs.length) {
          this.selectedColorNub = this.colorNubs.length - 1
        }
      }
    },
    addColor(x, y) {
      if (isNaN(x)) {
        x = Math.random() * 100
      }
      if (isNaN(y)) {
        y = Math.random() * 100
      }
      this.colorNubs.push({
        x: x,
        y: y,
        color: {
          h: Math.random() * 360, //this.accentHue(),
          s: 60.215,
          v: 93,
        },
      })
    },
  },
  mounted() {
    this.addColor(50, 50)
  },
}
</script>

<style lang="scss" scoped>
.topcontrols {
  @include reset;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: $size-fineprint;
  width: 100%;
}
</style>

<docs>
  ```jsx
  <div style="width: 250px">
    <ColorPicker/>
  </div>

  ```
</docs>
