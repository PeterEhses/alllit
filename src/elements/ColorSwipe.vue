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
  release: "0.0.3",
  props: {
    /**
     * should the color wheel spin while dragging or only change on mouse release?
     */
    wheel: {
      type: Boolean,
      default: false,
    },
    /**
     * gradient direction in deg, this one's useless right now
     */
    direction: {
      type: Number,
      default: 0,
    },
    /**
     * initial center/active value as hsl hue in deg. component will emit new value once changed by user
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
      c_oldval: this.value,
    }
  },
  computed: {
    primaryColor: function() {
      if (this.c_value) {
        /**
         *  new Color Value
         *
         * @event colorchange
         */
        this.$emit("colorchange", this.c_value)
        if (!this.wheel) {
          return this.c_oldval
        }
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
        if (mouseupEvent.type == "mouseup") {
          this._hasdragged = false
          return
        }
        // click before drag / up is registered
        if (mouseupEvent.movementX != 0 || mouseupEvent.movementY != 0 || this._hasdragged) {
          // mouse has moved
          this._hasdragged = true
          let newval
          if (this.wheel) {
            newval = this.c_value + mouseupEvent.movementY * (360 / this.$el.clientHeight)
            newval = ((newval % 360) + 360) % 360
          } else {
            let deriv = mouseupEvent.offsetY - this.$el.clientHeight / 2
            let deg = deriv * (360 / this.$el.clientHeight)
            newval = (((this.c_oldval - deg) % 360) + 360) % 360
          }

          this.c_value = newval
        } else {
          let deriv = mouseupEvent.offsetY - this.$el.clientHeight / 2
          let deg = deriv * (360 / this.$el.clientHeight)
          let newval = (((this.c_value - deg) % 360) + 360) % 360
          this.c_value = newval
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
      this.mouseEval(e)
    })
    this.$el.addEventListener("mousemove", e => {
      this.mouseEval(e)
    })
    this.$el.addEventListener("mouseup", e => {
      this.c_oldval = this.c_value
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
  ### Appears on ListChips to pick out a marking color
  ```jsx
  <div >
  <ColorSwipe :wheel='true'/> // optional flag for wheel-like rotation
  <ColorSwipe :value="150"/> // different starting value
  <ColorSwipe :value="500"/> // starting value above 360 deg possible. will be corrected to withing positive 360deg
  <ColorSwipe :value="-100"/> // starting val below 0 deg possible, will be corrected to within positive 360 deg
  </div>
  ```

  ```vue
  <template>
    <div :style="{background: bgcol, width: '100%'}">
      <ColorSwipe v-on:colorchange="colorSet"/>
    </div>
  </template>

  <script>
    export default{
      data: function(){
        return {
          bgcolor: 0
        }
      },
      computed: {
        bgcol: function(){

          return "hsla(" + this.bgcolor + ", 80%, 65%, 1)" // use returned color value in some way
        }
      },
      methods: {
        colorSet(val){ // react to emitted event, e.g. store locally or transform otherwise
          this.bgcolor = val;
        }
      }
    }
  </script>
  ```
</docs>
