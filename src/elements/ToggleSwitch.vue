<template>
  <label :class="[{ hasicon: hasicon, upright: upright, onoff: onoff }, 'switch']">
    <input type="checkbox" v-model="enabled" />
    <span class="slider">
      <span class="handle"
        ><p>{{ icon }}</p></span
      >
    </span>
  </label>
</template>

<script>
export default {
  name: "ToggleSwitch",
  status: "prototype",
  release: "0.0.1",
  props: {
    /**
     * switch value, either true or false. bind with v-model
     */
    value: {
      type: Boolean,
      default: true,
    },
    /**
     * is this an on / off or state/state slider?
     */
    onoff: {
      type: Boolean,
      default: false,
    },
    /**
     * rotate upright
     */
    upright: {
      type: Boolean,
      default: false,
    },
    /**
     * whether to display an icon if on/off state is on
     */
    hasicon: {
      type: Boolean,
      default: false,
    },
  },
  data: function() {
    return {
      enabled: this.value,
    }
  },
  computed: {
    icon: function() {
      if (this.hasicon) {
        if (this.enabled) {
          return "✔"
        } else {
          return "x"
        }
      } else {
        return ""
      }
    },
  },
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
.hasicon {
  &.switch {
    margin: 0 $space-m;
    & input:checked + .slider .handle {
      background-color: var(--primary-color);
    }
  }
  & .slider {
    & .handle {
      background-color: $offblack;
      top: -0.2em;
      left: -0.2em;
      height: 1em;
      width: 1em;
      & p {
        margin: 0;
        padding: 0;
        text-align: center;
        color: $offwhite;
      }
    }
  }
}

.upright {
  &.switch {
    transform: rotate(-90deg); //  translateX(.1em)
    margin: 0 -$space-l;
  }
}

.onoff {
  &.switch {
    & input:checked + .slider .handle {
      background-color: var(--primary-color);
    }
  }
  & .slider {
    & .handle {
      background-color: $offblack;
    }
  }
}

.switch {
  margin: 0 $space-l;
  box-sizing: border-box;
  position: relative;
  top: 0.3em;
  display: inline-block;
  //background: red;
  width: 1.5em;
  height: 0.75em;
  border-radius: $radius-force-circle;
  border: $space-xs solid $gray5;

  & input {
    width: 0;
    height: 0;
    opacity: 0;

    &:checked {
      & + .slider .handle {
        // -webkit-transform: translateX(calc(.65em + #{$space-xs*1}));
        // -ms-transform: translateX(calc(.65em + #{$space-xs*1}));
        // transform: translateX(calc(.65em + #{$space-xs*1}));
        -webkit-transform: translateX(0.75em);
        -ms-transform: translateX(0.75em);
        transform: translateX(0.75em);
        //background-color: var(--secondary-color);
      }
    }
  }
}
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  -webkit-transition: $duration-quickly;
  transition: $duration-quickly;
  & .handle {
    display: flex;
    justify-content: center;
    align-content: center;
    position: absolute;
    content: "";
    height: calc(0.75em - #{$space-xs * 2}); // magic numbers where do they come from
    width: calc(0.75em - #{$space-xs * 2});
    left: 0; //$space-xs / 2
    bottom: 0; //$space-xs / 2
    background-color: var(--primary-color);
    border-radius: $radius-force-circle;
    -webkit-transition: 0.4s;
    transition: 0.4s;
  }
}
</style>

<docs>
  ```vue
  <template>
    <div class="">
      <h2>This is some huge Text <ToggleSwitch :onoff="true" :upright="true"/></h2>
      <label>This is some Text <ToggleSwitch v-model="val" :onoff="true" :hasicon="true" /> {{val}} </label>
      <p :style="{'font-size': '12px'}">This is some small Text <ToggleSwitch/></p>
    </div>
  </template>

  <script>
  export default {
    data: function(){
      return {
        val: false
      }
    }
  }
  </script>

  <style lang="css" scoped>
  </style>



  ```
</docs>
