<template>
  <section :class="type">
    <slot>content</slot>
    <p>hi!</p>
  </section>
</template>

<script>
/**
 * the card container is an empty canvas for any card related components
 */
export default {
  name: "Container",
  status: "prototype",
  release: "0.0.1",
  props: {
    type: {
      type: String,
      required: true,
      default: "card",
      validator: function(value) {
        return ["card", "outline"].indexOf(value) !== -1
      },
    },
  },
}
</script>

<style lang="scss" scoped>
.card {
  min-width: $card-min-width;
  min-height: $card-min-height;
  background-color: $offwhite;
  margin: $space-s;
  border-radius: $radius-s;
  box-shadow: $shadow-light;
}

.outline {
  box-sizing: border-box;
  background-color: rgba(0, 0, 0, 0);
  border: $space-s solid $gray2;
  border-radius: $radius-m;
}
</style>

<docs>

### Container Type:

  ```jsx
  <div>
    <Container type="card"/>
  </div>
  ```
  ### Outline Type:

  ```jsx
  <div>
    <Container type="outline"/>
  </div>
  ```
</docs>
