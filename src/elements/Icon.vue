<template>
  <component :is="type" :aria-label="ariaLabel" :class="['icon', size]" v-html="dynamicSvg" />
</template>

<script>
const req = require.context("@/assets/icons/", true, /^\.\/.*\.svg$/)
/**
 * Icons are used to visually communicate core parts of the product and
 * available actions. They can act as wayfinding tools to help users more
 * easily understand where they are in the product.
 */
export default {
  name: "Icon",
  status: "prototype",
  release: "0.0.3",
  props: {
    /**
     * The name of the icon to display.
     */
    name: {
      required: true,
      default: "settings",
    },
    /**
     * The fill color of the SVG icon.
     */
    fill: {
      type: String,
      default: "currentColor",
    },
    /**
     * Descriptive text to be read to screenreaders.
     */
    ariaLabel: {
      type: String,
      default: "icon",
    },
    /**
     * The html element name used for the icon.
     */
    type: {
      type: String,
      default: "span",
    },
    /**
     * The size of the icon. Defaults to medium.
     * `small, medium, large or inline to dynamically resize to 1em`
     * @values small, medium, large, inline
     */
    size: {
      type: String,
      default: "medium",
      validator: value => {
        return value.match(/(small|medium|large|inline)/)
      },
    },
  },
  data() {
    return {}
  },
  computed: {
    dynamicSvg: function() {
      return req("./" + this.name + ".svg").replace(/^<svg /, `<svg style="fill: ${this.fill}" `)
    },
  },
}
</script>

<style lang="scss">
// This is here just to provide defaults if the original tokens are removed.
// Can be removed once you’re ready to start defining your own sizes.
@import "../../docs/docs.tokens.scss";
// We don’t want to use scoped since these styles need to cascade down to SVGs.
// We also want to be able to style .icon inside buttons etc.
.icon {
  transition: all $duration-quickly;
  @include reset;
  &.large svg {
    width: $size_3;
    height: $size_3;
  }
  &.medium svg {
    width: $size_4;
    height: $size_4;
  }
  &.small svg {
    width: $size_paragraph;
    height: $size_paragraph;
  }
  &.inline svg {
    position: relative;
    top: 0.15em;
    width: 1em;
    height: 1em;
  }
}
</style>

<docs>
  ```jsx
  <div style="font-size: 1.5em;">
    <Icon name="ready" aria-label="Component is ready" fill="#ED5C79" />
    <Icon name="review" fill="#F2E7A9" />
    <Icon name="deprecated" fill="rgb(255,0,0)" />
    <Icon name="prototype" fill="#090709" />
    <hr/>
    <Icon name="add" size="inline" fill="var(--primary-color)" />
    <Icon name="subtract" size="inline" fill="#090709" />
    <hr/>
    <Icon name="checkbox" size="inline" fill="#090709" />
    <Icon name="checkbox_checked" size="inline" fill="#090709" />
    <Icon name="checkmark" size="inline" fill="#090709" />
    <br/>
    <Icon name="lock_locked" size="inline" fill="#090709" />
    <Icon name="lock_open" size="inline" fill="#090709" />
    <br/>
    <Icon name="edit" size="inline" fill="#090709" />
    <Icon name="folder" size="inline" fill="#090709" />

    <Icon name="search" size="inline" fill="#090709" />
    <Icon name="template" size="inline" fill="#090709" />
    <hr/>
    <Icon name="close" size="inline" fill="#090709" />
    <Icon name="fullwindow" size="inline" fill="#090709" />
    <Icon name="minimize" size="inline" fill="#090709" />
    <Icon name="windowed" size="inline" fill="#090709" />
    <br/>
    <Icon name="drag" size="inline" fill="#090709" />
    <Icon name="foldout" size="inline" fill="#090709" />
    <Icon name="meatballs" size="inline" fill="#090709" />










  </div>
  ```
</docs>
