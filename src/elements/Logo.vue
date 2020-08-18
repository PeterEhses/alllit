<template>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 167.99 66">
    <defs>
      <filter id="f1" x="-100%" y="-100%" width="300%" height="300%">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" />
      </filter>
    </defs>
    <path
      d="M918.76,618.5a5.5,5.5,0,0,0-5.5,5.5v.19a24,24,0,1,0,.08,38.7,5.5,5.5,0,0,0,10.92-.89V624A5.5,5.5,0,0,0,918.76,618.5Zm-19.5,38.84a13.85,13.85,0,1,1,13.85-13.84A13.84,13.84,0,0,1,899.26,657.34Z"
      transform="translate(-875.27 -601.5)"
    />
    <g>
      <circle cx="130.49" cy="5.5" r="5.5" />
      <path
        d="M1005.76,618.5a5.5,5.5,0,0,0-5.5,5.5v38a5.5,5.5,0,0,0,11,0V624A5.5,5.5,0,0,0,1005.76,618.5Z"
        transform="translate(-875.27 -601.5)"
      />
    </g>
    <path
      d="M1038.26,656.52v0h-6v-27h6v0a5.5,5.5,0,0,0,0-11v0h-6V607a5.5,5.5,0,0,0-11,0v49.5a11,11,0,0,0,11,11h6v0a5.5,5.5,0,0,0,0-11Z"
      transform="translate(-875.27 -601.5)"
    />
    <rect :fill="secondary" filter="url(#f1)" x="102.99" width="11" height="66" rx="5.5" />
    <rect :fill="primary" filter="url(#f1)" x="59.99" width="11" height="66" rx="5.5" />
    <rect :fill="primary" filter="url(#f1)" x="80.99" width="11" height="66" rx="5.5" />
    <rect
      :fill="secondary"
      fill-opacity="0.5"
      filter="url(#f1)"
      x="80.99"
      width="11"
      height="66"
      rx="5.5"
    />
    <rect :fill="secondary" x="102.99" width="11" height="66" rx="5.5" />
    <rect :fill="primary" x="59.99" width="11" height="66" rx="5.5" />
    <rect :fill="primary" x="80.99" width="11" height="66" rx="5.5" />
    <rect :fill="secondary" fill-opacity="0.5" x="80.99" width="11" height="66" rx="5.5" />
  </svg>
</template>

<script>
export default {
  name: "Logo",
  status: "prototype",
  release: "0.0.1",
  data() {
    return {
      primary: "",
      secondary: "",
      colors: ["red", "green", "blue"],
    }
  },
  methods: {
    updateColors() {
      this.secondary = this.secondaryColor()
      this.primary = this.primaryColor()
    },
    primaryColor() {
      return getComputedStyle(this.$el).getPropertyValue("--primary-color")
    },
    secondaryColor() {
      return getComputedStyle(this.$el).getPropertyValue("--secondary-color")
    },
  },
  mounted() {
    this.updateColors()
    console.log(this.colors)
  },
}
</script>

<style lang="scss" scoped>
svg {
  overflow: visible;
}
</style>

<docs>
  ```jsx
  <div style="width: 200px">
    <Logo/>
  </div>
  ```
</docs>
