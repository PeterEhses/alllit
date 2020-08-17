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
    <div class="colorselect">
      <div class="valuesSelect">
        <ColorValues :color="activeColor" conversionType="CYMK" />
        <ColorValues :color="activeColor" conversionType="RGB" />
        <ColorValues :color="activeColor" conversionType="HSV" />
      </div>
      <div class="svselect"><SVColorPicker :color="activeColor" @dragged="e => setColor" /></div>
      <div class="hselect"><HueColorSlider :color="activeColor" @dragged="e => setColor" /></div>
    </div>
  </div>
</template>

<script>
import Icon from "@/elements/Icon.vue"
import TwoStateSwitch from "@/patterns/TwoStateSwitch.vue"
import VoronoiColorXYSlider from "@/patterns/VoronoiColorXYSlider.vue"
import SVColorPicker from "@/patterns/SVColorPicker.vue"
import HueColorSlider from "@/patterns/HueColorSlider.vue"
import ColorValues from "@/elements/ColorValues.vue"
export default {
  name: "ColorPicker",
  status: "prototype",
  release: "0.0.1",
  components: {
    Icon,
    TwoStateSwitch,
    VoronoiColorXYSlider,
    SVColorPicker,
    HueColorSlider,
    ColorValues,
  },
  props: {
    /**
     * colors object
     */
    colors: {
      type: Array,
      default: [],
    },
    /**
     * dimensionality
     */
    twoDim: {
      type: Boolean,
      default: false,
    },
    /**
     * colors object
     */
    gradientLerp: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      selectedColorNub: 0,
      oneDimensional: this.twoDim,
      lerpModeBool: this.gradientLerp,
      colorNubs: this.colors,
      newColor: {
        h: 0,
        s: 60.215,
        v: 93,
      },
    }
  },
  computed: {
    activeColor() {
      if (this.selectedColorNub < this.colorNubs.length && this.selectedColorNub >= 0) {
        return this.colorNubs[this.selectedColorNub].color
      } else {
        return this.newColor
      }
    },
    lerpMode() {
      if (this.lerpModeBool) {
        return "RGB"
      } else {
        return "nearest"
      }
    },
  },
  watch: {
    color: {
      handler() {
        this.colorNubs = this.color
      },
      deep: true,
    },
    colorNubs: {
      handler() {
        /**
         * color points change
         * @event colors-change
         * @property {array} colorpoints
         */
        this.$emit("colors-change", this.colorNubs)
      },
      deep: true,
    },
    twoDim() {
      this.oneDimensional = this.twoDim
    },
    gradientLerp() {
      this.lerpModeBool = this.gradientLerp
    },
    oneDimensional() {
      /**
       * dimension mode  change
       * @event dimension-change
       * @property {boolean} dimensionality
       */
      this.$emit("dimension-change", this.oneDimensional)
    },
    lerpModeBool() {
      /**
       * lerp mode  change
       * @event lerp-change
       * @property {boolean} lerpity
       */
      this.$emit("lerp-change", this.lerpModeBool)
    },
  },
  methods: {
    setColor(c) {
      if (this.selectedColorNub < this.colorNubs.length && this.selectedColorNub >= 0) {
        this.colorNubs[this.selectedColorNub].color = c
      } else {
        this.newColor = c
      }
    },
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
          h: this.newColor.h, //this.accentHue(),
          s: this.newColor.s,
          v: this.newColor.v,
        },
      })
    },
  },
  mounted() {
    this.newColor.h = this.accentHue()
    if (this.colorNubs.length < 1) {
      this.addColor(50, 50)
    }
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
.colorselect {
  margin-top: $space-l * 2;
  display: flex;
  align-items: stretch;
}
.svselect {
  flex-grow: 1;
  width: 66%;
}
.hselect {
  margin-left: $space-l * 2;
  height: auto;
}
.valuesSelect {
  margin-right: $space-l * 2;
  flex-grow: 0;
}
</style>

<docs>
  ```jsx
  <div style="width: 250px">
    <ColorPicker/>
  </div>

  ```
</docs>
