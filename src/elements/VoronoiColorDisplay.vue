<template>
  <div class="container" :style="{ height: containerHeight }">
    <canvas class="canvas" ref="canvas"></canvas>
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
     * color and location objects as array
     */
    colorSteps: {
      type: Array,
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
    colorStepsAbs: function() {
      let colorStepsAbs = []
      let that = this
      this.colorSteps.forEach(cs => {
        colorStepsAbs.push({
          x: Math.round((cs.y * that.dimensions.width) / 100), // WHY DO I HAVE TO SWITCH THIS IT DOESN'T MAKE SENSE
          y: Math.round((cs.x * that.dimensions.height) / 100),
          color: cs.color,
        })
      })
      return colorStepsAbs
    },
    stetcho: function() {
      if (this.stretchHeight) {
        return this.stretchHeight
      } else {
        return this.width / this.length
      }
      // stretchy boi either copied from the stretchy prop boi or computed from the length and width.
    },
    canvas: function() {
      return this.$refs.canvas.getContext("2d")
    },
    containerHeight: function() {
      return this.dimensions.width * this.stetcho + "px"
    },
    gradientData: function() {
      let that = this
      let xcs = this.colorStepsAbs.map(p => p.x)
      let ycs = this.colorStepsAbs.map(p => p.y)
      let xmin = Math.min(...xcs)
      let xmax = Math.max(...xcs)
      let ymin = Math.min(...ycs)
      let ymax = Math.max(...ycs)
      return {
        xcs: xcs,
        ycs: ycs,
        xmin: xmin,
        xmax: xmax,
        ymin: ymin,
        ymax: ymax,
      }
    },
  },
  watch: {
    lerpMode() {
      this.gradientUpdate()
    },
    /**
     * redraw canvas on gradient data change i.e. aspect, number / position / color of points as in everything
     */
    colorStepsAbs: {
      handler() {
        this.gradientUpdate()
      },
      deep: true,
    },
    /**
     * handle dimension change. update canvas dimensions and implicitly trigger redraw
     */
    dimensions: {
      handler() {
        this.canvas.width = this.dimensions.width
        this.canvas.height = this.dimensions.height
        this.$refs["canvas"].width = this.dimensions.width
        this.$refs["canvas"].height = this.dimensions.height
        /**
         * size event
         *
         * @event size-notice
         * @property {object} dimensions
         */
        this.$emit("size-notice", this.dimensions)
        this.gradientUpdate()
      },
      deep: true,
    },
  },
  methods: {
    gradientUpdate() {
      for (let x = 0; x < this.dimensions.width; x++) {
        for (let y = 0; y < this.dimensions.height; y++) {
          let mixColor = this.getColorMix({
            x: x,
            y: y,
          })
          this.canvas.fillStyle = mixColor
          this.canvas.fillRect(x, y, 1, 1)
        }
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
    paddingleft0(v, v_length) {
      while (v.length < v_length) {
        v = "0" + v
      }
      return v
    },
    getWeightedColorMix(ratios) {
      let r = 0
      let g = 0
      let b = 0
      let h = 0,
        s = 0,
        v = 0

      if (this.lerpMode == "nearest") {
        // lerpMode="nearest"
        let highestInd = 0
        let highestVal = 0
        for (let [ind, val] of ratios.entries()) {
          if (highestVal <= val) {
            highestVal = val
            highestInd = ind
          }
        }
        //console.log(highestInd,this.colorStepsAbs[highestInd].color.h)
        let colBoi = this.HSVToRGB(this.colorStepsAbs[highestInd].color)
        r = colBoi.r
        g = colBoi.g
        b = colBoi.b
      } else {
        for (let [ind, point] of this.colorStepsAbs.entries()) {
          if (this.lerpMode == "HSV") {
            h += point.color.h * ratios[ind]
            s += point.color.s * ratios[ind]
            v += point.color.v * ratios[ind]
          } else {
            let newCol = this.HSVToRGB(point.color)
            r += Math.round(newCol.r * ratios[ind])
            g += Math.round(newCol.g * ratios[ind])
            b += Math.round(newCol.b * ratios[ind])
          }
        }

        if (this.lerpMode == "HSV") {
          let newCol = this.HSVToRGB(h, s, v)
          r = Math.round(newCol.r)
          g = Math.round(newCol.g)
          b = Math.round(newCol.b)
        }
      }
      let result =
        "#" +
        this.paddingleft0(r.toString(16), 2) +
        this.paddingleft0(g.toString(16), 2) +
        this.paddingleft0(b.toString(16), 2)
      return result
    },
    getProjectionDistance(a, b, c) {
      const k2 = b.x * b.x - b.x * a.x + b.y * b.y - b.y * a.y
      const k1 = a.x * a.x - b.x * a.x + a.y * a.y - b.y * a.y
      const ab2 = (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y)
      const kcom = c.x * (a.x - b.x) + c.y * (a.y - b.y)
      const d1 = (k1 - kcom) / ab2
      const d2 = (k2 + kcom) / ab2
      return {
        d1,
        d2,
      }
    },
    limit01(value) {
      if (value < 0) {
        return 0
      }
      if (value > 1) {
        return 1
      }
      return value
    },
    eucliDist(p1, p2) {
      let a = p1.x - p2.x
      let b = p1.y - p2.y
      let dist = Math.sqrt(a * a + b * b)
      dist = Math.abs(dist)
      return dist
    },
    /**
     * Given some points with color attached, calculate the color for a new point
     *   p The new point position {x: number, y: number}
     *  hex color string -- The weighted color mix
     */
    getColorMix(pos) {
      if (this.colorStepsAbs.length == 1) {
        let newCol = this.HSVToRGB(this.colorStepsAbs[0].color)
        let result =
          "#" +
          this.paddingleft0(newCol.r.toString(16), 2) +
          this.paddingleft0(newCol.g.toString(16), 2) +
          this.paddingleft0(newCol.b.toString(16), 2)
        return result
      } else if (this.lerpMode == "nearest") {
        let colorDistances = new Array(this.colorStepsAbs.length)
        colorDistances.fill(0)
        let shortestDist = 1000000
        let returnInd = 0
        for (let [ind, point] of this.colorStepsAbs.entries()) {
          colorDistances[ind] = this.eucliDist(point, pos)
          if (colorDistances[ind] < shortestDist) {
            shortestDist = colorDistances[ind]
            returnInd = ind
          }
        }
        let newCol = this.HSVToRGB(this.colorStepsAbs[returnInd].color)
        let result =
          "#" +
          this.paddingleft0(newCol.r.toString(16), 2) +
          this.paddingleft0(newCol.g.toString(16), 2) +
          this.paddingleft0(newCol.b.toString(16), 2)
        return result
      } else {
        let colorRatios = new Array(this.colorStepsAbs.length)
        colorRatios.fill(1)

        for (let [ind1, point1] of this.colorStepsAbs.entries()) {
          for (let [ind2, point2] of this.colorStepsAbs.entries()) {
            if (ind1 != ind2) {
              let d = this.getProjectionDistance(point1, point2, pos)
              colorRatios[ind1] *= this.limit01(d.d2)
            }
          }
        }
        let totalRatiosSum = 0
        colorRatios.forEach(c => {
          totalRatiosSum += c
        })

        colorRatios.forEach((c, i) => (colorRatios[i] /= totalRatiosSum))
        let c = this.getWeightedColorMix(colorRatios)
        return c
      }
    },

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
      //this.canvas.clearRect(0, 0, 400, 200)

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
    let config = {
      attributes: true,
    }
    this.observer = new ResizeObserver(this.callbackObserver)
    this.observer.observe(targetNode, config)
  },
  destroyed() {
    this.observer.disconnect()
  },
}
</script>

<style lang="scss" scoped>
.canvas,
.container {
  @include reset;
}
</style>

<docs>
  ```jsx
  <div style="width: 200px">
    <VoronoiColorDisplay lerpMode="HSV" :colorSteps="[
      {
          x: 0,
          y: 50,
          color: {h: 60, s: 0, v: 0}
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
    ]"/>
    <hr/>
    <VoronoiColorDisplay lerpMode="RGB" :colorSteps="[
      {
          x: 0,
          y: 50,
          color: {h: 60, s: 0, v: 0}
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
    ]"/>
    <hr/>
    <VoronoiColorDisplay lerpMode="nearest" :colorSteps="[
      {
          x: 0,
          y: 50,
          color: {h: 60, s: 0, v: 0}
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
    ]"/>
  </div>

  ```
</docs>
