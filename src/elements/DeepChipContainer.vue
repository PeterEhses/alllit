<template>
  <div id="outer">
    <div id="inner"><slot></slot></div>
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
  margin: $space-s;
  padding: $space-s $space-l $space-s $space-s;
  height: $chip-height;
  background-color: $primary;
  background: linear-gradient(90deg, $gray2 66%, var(--primary-color) 100%);
  border-radius: $radius-force-circle 0 0 $radius-force-circle;
}
#inner {
  box-sizing: border-box;
  height: 100%;
  background-color: $offwhite;
  padding: $space-l 50%;

  border-radius: $radius-force-circle;
}
</style>

<docs>
  ```jsx
  <div>
    <DeepChipContainer/>
  </div>
  ```
</docs>
