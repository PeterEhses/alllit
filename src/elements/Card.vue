<template>
  <section>
    <slot>content</slot>
    <p>hi!</p>
  </section>
</template>

<script>
/**
 * the card container is an empty canvas for any card related components
 */
export default {
  name: "CardContainer",
  status: "prototype",
  release: "0.0.1",
  props: {},
}
</script>

<style lang="scss" scoped>
section {
  min-width: $card-min-width;
  min-height: $card-min-height;
  background-color: $offwhite;
  margin: $space-s;
  border-radius: $radius-s;
  box-shadow: $shadow-light;
}
</style>

<docs>
  ```jsx
  <div>
    <CardContainer/>
  </div>
  ```
</docs>
