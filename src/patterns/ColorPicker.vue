<template>
  <div class="colorpicker">
    <div class="topcontrols">
      <span>
        <Icon name="add" fill="var(--text-color)" size="inline" />
        <Icon name="add" fill="var(--text-color)" size="inline" />
      </span>
      <TwoStateSwitch left="1D" right="2D" />
      <TwoStateSwitch left="nearest" right="gradient" />
    </div>
  </div>
</template>

<script>
import Icon from "@/elements/Icon.vue"
import TwoStateSwitch from "@/patterns/TwoStateSwitch.vue"
export default {
  name: "ColorPicker",
  status: "prototype",
  release: "0.0.1",
  components: {
    Icon,
    TwoStateSwitch,
  },
  props: {
    /**
     * dummy
     */
    /**
     * The fill color of the SVG icon.
     */
    dummy: {
      type: String,
      default: "dummy",
    },
  },
  data() {
    return {}
  },
}
</script>

<style lang="scss" scoped>
.topcontrols {
  @include reset;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: $size-fineprint;
  width: 100%;
}
</style>

<docs>
  ```jsx
  <ColorPicker/>
  ```
</docs>
