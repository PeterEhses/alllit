<template>
  <div class="base"></div>
</template>

<script>
export default {
  name: "SVColorPicker",
  status: "prototype",
  release: "0.0.1",
}
</script>

<style lang="scss" scoped>
.base {
  width: 260px;
  height: 260px;
  background: linear-gradient(0deg, rgba(0, 0, 0, 1), rgba(255, 255, 255, 0)),
    linear-gradient(-90deg, rgba(255, 0, 0, 1), rgba(255, 255, 255, 1));
}
</style>

<docs>
  ```jsx
  <SVColorPicker/>
  ```
</docs>
