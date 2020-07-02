<template>
  <div id="outer">
    <div id="handle">
      <div></div>
      <div></div>
    </div>
    <p><slot /></p>
    <div id="hole"></div>
  </div>
</template>

<script>
/**
 * container for all sorts of chips. todo: un-responsive corner radii, open / close functionality, etc. etc.
 */
export default {
  name: "Tag",
  status: "prototype",
  release: "0.0.1",
  props: {},
}
</script>

<style lang="scss" scoped>
#outer {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
  position: relative;
  align-items: center;
  padding: $space-s;
  box-shadow: $small-shadow-light;
  height: $chip-height;
  width: fit-content;
  min-width: $chip-height;
  background-color: $primary;
  background: linear-gradient(90deg, var(--secondary-color) 0%, var(--primary-color) 100%);
  border-radius: 0 $radius-force-circle $radius-force-circle 0;
}
p {
  font-weight: $weight-bold;
  font-family: $font-default;
  font-size: $size-paragraph;
  padding: 0;
  margin: 0 $space-s;
}
#hole {
  position: relative;
  width: $chip-height/2;
  height: $chip-height/2;
  margin: $chip-height/4;
  background-color: $offwhite;
  box-shadow: inset $small-shadow-light;
  border-radius: $radius-force-circle;
}
#handle {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
  position: relative;
  align-items: center;
  height: 100%;
  margin: 0 $space-s;
  & :nth-child(n) {
    height: 100%;
    width: $space-s;
    background-color: $offwhite;
    margin: $space-xs;
    border-radius: $radius-force-circle;
  }
  & :nth-child(1) {
    height: 70%;
  }
  & :nth-child(2) {
    height: 50%;
  }
}
// style="border: 1px solid red;"
</style>

<docs>
  ```jsx
  <div >
    <Tag>Device List</Tag>
  </div>
  ```
</docs>
