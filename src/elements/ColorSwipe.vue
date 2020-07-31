<template>
  <div id="swiper" :style="style"><!-- hi {{primaryColor}} --></div>
</template>

<script>
/**
 * the color picker that appears on deep chips in sidemenu mode
 */
import designTokens from "@/assets/tokens/tokens.raw.json"
//console.dir(designTokens);
export default {
  name: "ColorSwipe",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * gradient direction in deg, this one's useless right now
     */
    direction: {
      type: Number,
      default: 0,
    },
    /**
     * center/active value as hsl hue in deg
     */
    value: {
      type: Number,
      default: null,
    },
    /**
     * granularity of gradient. higher value results in longer gradient string, possibly processing time? higher value gives more precise gradient. 15 seems ok.
     */
    granularity: {
      type: Number,
      default: 15,
    },
  },
  data: function() {
    return {
      el: null,
      c_value: this.value,
    }
  },
  computed: {
    primaryColor: function() {
      if (this.c_value) {
        return this.c_value
      } else {
        if (this.el) {
          return parseInt(getComputedStyle(this.el).getPropertyValue("--primary-hue"))
        } else {
          return ""
        }
      }
    },
    gradient: function() {
      let grad = "linear-gradient("
      grad += this.direction + "deg, "
      let granularity = this.granularity
      for (let i = 0; i < granularity; i++) {
        grad += this.getFromHue((180 + i * (360 / granularity) + this.primaryColor) % 360) + ", "
      }
      grad += this.getFromHue((540 + this.primaryColor) % 360)
      grad += ")"
      return grad
    },
    style: function() {
      return {
        background: this.gradient,
      }
    },
  },
  methods: {
    getFromHue: function(val) {
      return "hsla(" + val + ", 80%, 65%, 1)"
    },
    mouseEval(mouseupEvent) {
      // this will ahve a bug where if mouse up is outside the component, mousemove will fire the following times the mouse is over the component
      let mousedownEvent = this.mousedownEvent
      if (this.clicking) {
        // click before drag / up is registered
        if (mouseupEvent.movementX != 0 || mouseupEvent.movementY != 0 || this._hasdragged) {
          // mouse has moved
          this._hasdragged = true
          console.log(mouseupEvent.movementY)
          this.c_value += mouseupEvent.movementY * (360 / this.$el.clientHeight)
        } else {
          let deriv = mouseupEvent.offsetY - this.$el.clientHeight / 2
          let deg = deriv * (360 / this.$el.clientHeight)
          let newval = (this.c_value - deg + 360) % 360
          this.c_value = newval
        }
        if (mouseupEvent.type == "mouseup") {
          this._hasdragged = false
        }
      }
    },
  },
  mounted: function() {
    //this.c_value = this.value;
    this.el = this.$el

    this.$el.addEventListener("mousedown", e => {
      this.clicking = true
      this.mousedownEvent = e
    })
    this.$el.addEventListener("mousemove", e => {
      this.mouseEval(e)
    })
    this.$el.addEventListener("mouseup", e => {
      this.mouseEval(e)
      this.clicking = false
    })
  },
}
</script>

<style lang="scss" scoped>
#swiper {
  height: $chip-height * 3;
  width: $icon_size;
  border-radius: $radius-force-circle;
  box-shadow: $shadow-light;
}
</style>

<docs>
  ```jsx
  <div >
  <ColorSwipe/> <ColorSwipe :value="150"/> <ColorSwipe :value="500"/> <ColorSwipe :value="-100"/>
  </div>
  ```
</docs>
