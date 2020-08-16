<template>
  <div class="sliderparent" :style="parentStyle">
    <input
      type="range"
      :min="min"
      :max="max"
      :step="step"
      class="slider"
      v-model="internal_value"
    />
  </div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"

export default {
  name: "Slider",
  status: "prototype",
  release: "0.0.2",
  props: {
    /**
     * The min value
     */
    min: {
      type: Number,
      default: 0,
    },
    /**
     * The max value
     */
    max: {
      type: Number,
      default: 1,
    },
    /**
     * width with unit. if no unit specified percent is assumed
     */
    width: {
      type: [String, Number],
      default: "10em",
    },
    /**
     * value, bind with v-model
     */
    value: {
      type: Number,
      default: 0,
    },
    /**
     * the sliding resolution / precision
     */
    step: {
      type: Number,
      default: 0.01,
    },
    /**
     * should handle color change in relation to handle position?
     */
    gradient: {
      type: Boolean,
      default: true,
    },
  },
  data: function() {
    return {
      internal_value: this.value,
    }
  },
  computed: {
    handleColor: function() {
      // return interpolated handle color
      if (this.gradient) {
        let lerpVal = 0
        if (this.internal_value > 0) {
          lerpVal = this.internal_value / this.max
        }

        let from = "var(--text-color)"
        let to = "var(--primary-color)"
        let fromperc = 0
        if (lerpVal != 0) {
          fromperc = -1000 * lerpVal
        }
        let toperc = 1000 + fromperc
        return (
          "linear-gradient(to bottom, " + from + " " + fromperc + "%, " + to + " " + toperc + "%)"
        )
      } else {
        return designTokens.props.offblack.value
      }
    },
    parentWidth: function() {
      // return width of parent element from props
      let w = ""
      if (typeof this.width === "string" || this.width instanceof String) {
        w = this.width
      } else if (!isNaN(this.width)) {
        w += this.width + "%"
      } else {
        w = "100%"
      }
      return w
    },
    parentStyle: function() {
      // collector for all parent styles
      return {
        width: this.parentWidth,
        "--handle-color": this.handleColor,
      }
    },
  },
  watch: {
    value: function() {
      this._value = this.value
    },
    internal_value: function() {
      this.$emit("input", this._value)
    },
  },
}
</script>

<style lang="scss" scoped>
.sliderparent {
  height: 1em;
  display: flex;
  align-items: center;
}
.slider {
  width: 100%;
  -webkit-appearance: none; // Override default CSS styles
  appearance: none;
  height: $space-s;
  background-color: $gray5;
  &::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    width: 1em; /* Set a specific slider handle width */
    height: 1em; /* Slider handle height */
    background: var(--handle-color); /* Green background */
    cursor: pointer; /* Cursor on hover */
    border-radius: $radius-force-circle;
  }
  &::-moz-range-thumb {
    width: 1em; /* Set a specific slider handle width */
    height: 1em; /* Slider handle height */
    background: var(--handle-color); /* Green background */
    cursor: pointer; /* Cursor on hover */
    border-radius: $radius-force-circle;
  }
}
</style>

<docs>
  ```jsx
  <Slider/>
  ```
</docs>
