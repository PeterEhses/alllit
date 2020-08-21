<template>
  <div class="vertstack">
    <label v-for="(value, name) in values" :key="name">
      <input type="number" :value="value" v-if="conversionType != 'HEX'" />
      <input type="text" :value="value" v-if="conversionType == 'HEX'" /> {{ name }}
    </label>
  </div>
</template>

<script>
export default {
  name: "ColorValues",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * input color
     */
    color: {
      type: Object,
      default: () => {
        return {
          h: 200,
          s: 100,
          v: 100,
        }
      },
    },
    conversionType: {
      type: String,
      default: "CYMK",
      validator: value => {
        return value.match(/(RGB|HSV|CYMK|HEX)/)
      },
    },
  },
  data() {
    return {
      internalColor: this.color,
    }
  },
  computed: {
    values() {
      switch (this.conversionType) {
        case "CYMK":
          return this.RGBToCYMK(this.HSVToRGB(this.internalColor))
          break

        case "RGB":
          return this.HSVToRGB(this.internalColor)
          break

        case "HSV":
          return this.cleanHSV(this.internalColor)
          break
        case "HEX":
          return this.RGBToHex(this.HSVToRGB(this.internalColor))
          break
      }
    },
  },
  watch: {
    color() {
      this.internalColor = this.color
    },
  },
  methods: {
    RGBToHex(r, g, b) {
      if (arguments.length === 1) {
        ;(r = arguments[0].r), (g = arguments[0].g), (b = arguments[0].b)
      }

      r = r.toString(16)
      g = g.toString(16)
      b = b.toString(16)

      if (r.length == 1) r = "0" + r
      if (g.length == 1) g = "0" + g
      if (b.length == 1) b = "0" + b

      return { hex: "#" + r + g + b }
    },
    cleanHSV(h, s, v) {
      if (arguments.length === 1) {
        ;(s = h.s), (v = h.v), (h = h.h)
      }
      return {
        h: Math.round(h),
        s: Math.round(s),
        v: Math.round(v),
      }
    },
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
    RGBToCYMK(r, g, b) {
      if (arguments.length === 1) {
        ;(r = arguments[0].r), (g = arguments[0].g), (b = arguments[0].b)
      }

      let c = 1 - r / 255
      let m = 1 - g / 255
      let y = 1 - b / 255

      c = isNaN(c) ? 1 : c
      m = isNaN(m) ? 1 : m
      y = isNaN(y) ? 1 : y

      let k = Math.min(c, Math.min(m, y))

      c = (c - k) / (1 - k)
      m = (m - k) / (1 - k)
      y = (y - k) / (1 - k)

      return {
        c: Math.round(c * 100),
        m: Math.round(m * 100),
        y: Math.round(y * 100),
        k: Math.round(k * 100),
      }
    },
  },
}
</script>

<style lang="scss" scoped>
label {
  text-transform: uppercase;
  display: block;
  font-size: $size-fineprint;
  font-weight: $weight-bold;
}
input {
  background: rgba(255, 255, 255, 0);
  text-align: right;
  width: 2em;
  font-size: $size-fineprint;
  border: none;
  border-bottom: $space-s solid $gray1;
}
input[type="text"] {
  width: 4.4em;
  text-transform: uppercase;
}
.vertstack + .vertstack {
  margin-top: $space-l * 2;
}
</style>

<docs>

### Shows the Color value in three different color models and in HEX form.
  #### this component is a stub. No reaction to user input is implemented.
  ```jsx
  <div class="">
    <ColorValues/>
    <ColorValues conversionType="RGB"/>
    <ColorValues conversionType="HSV"/>
    <ColorValues conversionType="HEX"/>
  </div>

  ```
</docs>
