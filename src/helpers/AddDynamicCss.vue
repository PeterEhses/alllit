<template>
  <div :style="cssVars"><slot /></div>
</template>

<script>
import designTokens from "@/assets/tokens/tokens.raw.json"
export default {
  name: "AddDynamicCss",
  data() {
    return {
      theme: {
        primary_color: 348,
        secondary_color: 60,
        text_color: designTokens.props.offblack.value,
        bg_primary: designTokens.props.offwhite.value,
        bg_2: designTokens.props.gray2.value,
        bg_4: designTokens.props.gray4.value,
      },
    }
  },
  render(h) {
    return h(div)
  },
  computed: {
    cssVars() {
      return {
        "--text-color": this.theme.text_color, //default text color. also in icons, etc.
        "--bg-primary": this.theme.bg_primary, // default background. it's behind most things
        "--bg-2": this.theme.bg_2, // gray 2 for light mode.
        "--bg-4": this.theme.bg_4, // gray 4 for light mode.
        "--primary-hue": this.theme.primary_color, // primary accent color hue. for smart math stuff
        "--primary-color": "hsla( var(--primary-hue), 80%, 65%, 1)", // primary color as hacky thing
        "--secondary-color": "hsla(" + this.theme.secondary_color + ", 66%, 79%, 1)",
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

/* width */
::-webkit-scrollbar {
  width: $space-m * 3;
}

/* Track */
::-webkit-scrollbar-track {
  background: transparent;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: var(--bg-2);
  margin: 2px;
  border: $space-m solid var(--bg-primary);
  border-radius: $radius-force-circle;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: var(--bg-4);
}

::-webkit-resizer,
::-webkit-scrollbar-button,
::-webkit-scrollbar-corner {
  display: none;
}
</style>
