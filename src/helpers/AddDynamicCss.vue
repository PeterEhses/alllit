<template>
  <div :style="cssVars"><slot /></div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"
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
        "--text-color": designTokens.props.offblack.value, //default text color. also in icons, etc.
        "--bg-primary": designTokens.props.offwhite.value, // default background. it's behind most things
        "--primary-hue": this.primary_color, // primary accent color hue. for smart math stuff
        "--primary-color": "hsla( var(--primary-hue), 80%, 65%, 1)", // primary color as hacky thing
        "--secondary-color": "hsla(" + this.secondary_color + ", 66%, 79%, 1)",
      }
    },
  },
}
</script>

<style lang="scss">
:root {
  font-weight: $weight-normal;
  font-family: $font-default;
  font-size: $size-paragraph;
}
input {
  font-weight: $weight-normal;
  font-family: $font-default;
  font-size: $size-paragraph;
  &::-webkit-outer-spin-button,
  &::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
}
input[type="number"] {
  -moz-appearance: textfield;
}
</style>
