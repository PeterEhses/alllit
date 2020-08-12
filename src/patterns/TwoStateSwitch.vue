<template>
  <div class="twostatecontainer">
    <label>
      <p :class="enabled ? '' : 'bolden'" :title="left">{{ left }}</p>
      <ToggleSwitch v-model="enabled" />
      <p :class="enabled ? 'bolden' : ''" :title="right">{{ right }}</p>
    </label>
  </div>
</template>

<script>
import ToggleSwitch from "@/elements/ToggleSwitch.vue"
export default {
  components: {
    ToggleSwitch,
  },
  name: "TwoStateSwitch",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * left slot content
     */
    left: {
      type: String,
      default: "left slot",
    },
    /**
     * right slot content
     */
    right: {
      type: String,
      default: "right slot",
    },
    /**
     * value. bind with v-model
     */
    value: {
      type: Boolean,
      default: false,
    },
  },
  data: function() {
    return {
      enabled: this.value,
    }
  },
  computed: {},
  watch: {
    value: function() {
      this.enabled = this.value
    },
    enabled: function() {
      this.$emit("input", this.enabled)
    },
  },
}
</script>

<style lang="scss" scoped>
p {
  @include prevent-user-select;
  &:before {
    display: block;
    content: attr(title);
    font-weight: $style-bold;
    height: 0;
    overflow: hidden;
    visibility: hidden;
  }
}
label {
  font-size: $size-fineprint;
  display: flex;
  align-items: baseline;
}

.bolden {
  font-weight: $style-bold;
}
</style>

<docs>
### a switch with two clickable texts either side of which the active one is bold
```vue
<template>
  <div>
    <TwoStateSwitch left="Linkes Twix" right="Rechtes Twix"/>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
</style>
```
</docs>
