<template>
  <div class="listchip">
    <div class="listchippre">
      <div class="preiconcircle"></div>
      <div class="preiconcircle"></div>
    </div>
    <DeepChipContainer :accentcolor="computedAccentColor">
      <div class="chipcontent">
        <div class="topbar">
          <div class="info">
            <strong>{{ id }}</strong>
            <p>{{ name }}</p>
          </div>

          <div class="nav">
            <Icon
              name="foldout"
              :class="[fouldout ? null : 'foldin']"
              size="inline"
              @click.native="fouldout = !fouldout"
            />
            <Icon name="meatballs" size="inline" />
            <div
              :class="['colorSwipeContain', colorSwipeActive ? null : 'hidecontent']"
              @mousedown="colorSwipeActive = true"
              @mouseup="colorSwipeActive = false"
            >
              <ColorSwipe @colorchange="e => (accentHue = e)" />
            </div>
          </div>
        </div>
        <div :class="['details', fouldout ? null : 'detailshidden']">
          <ColorPicker
            v-if="fouldout"
            :colors="colors"
            @colors-change="e => (colors = e)"
            :gradientLerp="lerpMode"
            @lerp-change="e => (lerpMode = e)"
            :twoDim="twoDim"
            @dimension-change="e => (twoDim = e)"
          />
        </div>
      </div>
    </DeepChipContainer>
  </div>
</template>

<script>
import DeepChipContainer from "@/elements/DeepChipContainer.vue"
import ColorSwipe from "@/elements/ColorSwipe.vue"
import ColorPicker from "@/patterns/ColorPicker.vue"
import Icon from "@/elements/Icon.vue"
export default {
  name: "ListChip",
  status: "prototype",
  release: "0.0.1",
  components: {
    DeepChipContainer,
    ColorSwipe,
    ColorPicker,
    Icon,
  },
  props: {
    name: {
      type: String,
      default: "noname",
    },
    id: {
      type: Number,
      default: -1,
    },
  },
  data() {
    return {
      fouldout: false,
      colorSwipeActive: false,
      twoDim: true,
      lerpMode: true,
      colors: [],
      accentHue: NaN,
    }
  },
  computed: {
    computedAccentColor() {
      if (isNaN(this.accentHue)) {
        return undefined
      } else {
        return "hsla( " + this.accentHue + ", 80%, 65%, 1)"
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.listchip {
  width: 100%;
  display: flex;
}
.listchippre {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: $size-paragraph + 0.5px;
  height: $chip-height;
}
.preiconcircle {
  width: $size-paragraph - $space-s;
  height: $size-paragraph - $space-s;
  border-radius: $radius-force-circle;
  background-color: $gray2;
}
.chipcontent {
  display: flex;
  flex-direction: column;
}
.topbar {
  height: $chip-height;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.info {
  display: flex;
  margin-left: 1em;
  & p {
    margin: 0;
  }
  & strong {
    text-align: right;
    //width: 2.5em; // if we want to have them all be the same size enable this
    margin-right: 0.5em;
  }
}
.nav {
  display: flex;
  align-items: center;
  & :nth-child(2) {
    margin-left: $space-l + $icon-size/2;
  }
  & :nth-child(3) {
    margin-left: $space-l;
  }
}
.colorSwipeContain {
  height: $chip-height;
  width: $icon-size;
  display: flex;
  align-items: center;
}
.foldin {
  transform: rotate(90deg);
}
.hidecontent {
  overflow: hidden;
  & :nth-child(n) {
    opacity: 0;
  }
}
.detailshidden {
  // doesn't work currently because height is auto by default and auto height doesnt animate. why
  height: 0;
  padding: 0 1em !important;
}
.details {
  box-sizing: border-box;
  max-width: 100%;
  width: calc(100% - 1em);
  //overflow: hidden;
  padding: 0 0 1em 1em;
}
</style>

<docs>
  ```jsx
  <div class="doc" style="width: 320px">
      <ListChip :id="999" :name="'allit'"/>
  </div>
  ```
</docs>
