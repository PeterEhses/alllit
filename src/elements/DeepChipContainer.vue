<template>
  <div class="outer" :style="{ background: color }">
    <div class="inner"></div>
    <div class="content"><slot></slot></div>
  </div>
</template>

<script>
/**
 * container for all sorts of chips. todo: un-responsive corner radii, open / close functionality, etc. etc.
 */
import designTokens from "@/assets/tokens/tokens.raw.json"

export default {
  name: "DeepChipContainer",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * color for righthand accent
     */
    accentcolor: {
      type: String,
      default: null,
      required: false,
    },
  },
  computed: {
    color: function() {
      let grad = "var(--primary-color)"
      if (this.accentcolor && this.accentcolor.length > 0) {
        grad = this.accentcolor
      }
      return "linear-gradient(90deg, " + designTokens.props.gray2.value + " 66%, " + grad + " 100%)"
    },
  },
}
</script>

<style lang="scss" scoped>
.outer {
  flex-grow: 1;
  position: relative;
  margin: 0;
  //padding: $space-s $space-l $space-s $space-s;
  width: auto;
  height: auto;
  background-color: $primary;
  background: linear-gradient(90deg, $gray2 66%, var(--primary-color) 100%);
  border-radius: $chip-height/2 0 0 $chip-height/2;
}
.inner {
  position: absolute;
  box-sizing: border-box;
  background-color: $offwhite;
  z-index: 0;
  border-radius: $chip-height/2-$space-s;
  top: $space-s;
  left: $space-s;
  bottom: $space-s;
  right: $space-l;
  width: auto;
}
.content {
  width: 100%;
  min-height: $chip-height;
  z-index: 2;
  position: relative;

  margin: 0;
  padding: 0;
}
</style>

<docs>
### deep chip works with single lines
  ```js
  <div>
    <DeepChipContainer accentcolor="#298cd4">hi</DeepChipContainer>
  </div>
  ```
  ---
### but expands to its dynamic open state if content calls for it
```js
<div>
  <DeepChipContainer>1<br/>2<br/>3<br/>4</DeepChipContainer>
</div>
```
### the deep chip is only a styled container with no padding so it needs to be filled with dynaic contents, clickable areas, etc.
```js
<div>
  <DeepChipContainer><div :style="{background: 'var(--secondary-color)', height: '1rem'}">hi again</div></DeepChipContainer>
</div>
```
</docs>
