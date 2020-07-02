<template>
  <div :style="cssVars"><slot /></div>
</template>

<script>
export default {
  name: "AddDynamicCss",
  data() {
    return {
      primary_color: 348,
      secondary_color: 60,
    }
  },
  render(h) {
    return h(div)
  },
  computed: {
    cssVars() {
      return {
        "--primary-color": "hsla(" + this.primary_color + ", 80%, 65%, 1)",
        "--secondary-color": "hsla(" + this.secondary_color + ", 66%, 79%, 1)",
      }
    },
  },
}
</script>

<style lang="scss"></style>
