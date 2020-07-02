<template>
  <div id="outer">
    <div id="inner"></div>
    <div id="content"><slot></slot></div>
  </div>
</template>

<script>
/**
 * container for all sorts of chips. todo: un-responsive corner radii, open / close functionality, etc. etc.
 */
export default {
  name: "DeepChipContainer",
  status: "prototype",
  release: "0.0.1",
  props: {},
}
</script>

<style lang="scss" scoped>
#outer {
  position: relative;
  margin: $space-s;
  //padding: $space-s $space-l $space-s $space-s;
  height: auto;
  background-color: $primary;
  background: linear-gradient(90deg, $gray2 66%, var(--primary-color) 100%);
  border-radius: $chip-height/2 0 0 $chip-height/2;
}
#inner {
  position: absolute;
  box-sizing: border-box;
  background-color: $offwhite;
  z-index: 0;
  border-radius: $chip-height/2-$space-s;
  top: $space-s;
  left: $space-s;
  bottom: $space-s;
  right: $space-l;
}
#content {
  width: 100%;
  min-height: $chip-height;
  z-index: 2;
  position: relative;

  margin: 0;
  padding: 0;
}
</style>

<docs>
  ```jsx
  <div>
    <DeepChipContainer>hi</DeepChipContainer>
  </div>
  ```
</docs>
