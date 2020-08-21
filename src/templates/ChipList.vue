<template>
  <Container type="card" class="devicecontainer">
    <div class="nav">
      <Tag class="tag">Device List</Tag>
      <div class="buttons">
        <Icon name="meatballs" size="inline" />
        <Icon name="close" size="inline" />
      </div>
    </div>
    <div class="devicelist">
      <ListChip v-for="(chip, id) in chips" :id="chip.id" :name="chip.name" :key="id" />
    </div>
  </Container>
</template>

<script>
import ListChip from "@/templates/ListChip.vue"
import Container from "@/elements/Container/Container.vue"
import Icon from "@/elements/Icon.vue"
import Tag from "@/elements/Tag.vue"
export default {
  name: "ChipList",
  status: "prototype",
  release: "0.0.1",
  components: {
    ListChip,
    Container,
    Tag,
    Icon,
  },
  props: {
    /**
     * chip data array. stub
     */
    chips: {
      type: Array,
      default: () => [],
    },
  },
}
</script>

<style lang="scss" scoped>
.nav {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: $safespace;
}
.buttons {
  position: relative;
  margin: $space-m;
  display: flex;
  font-size: 1rem;
  & > * {
    padding: $icon-padding;
  }
}
.tag {
  position: relative;
  top: $space-m;
  left: -$space-s;
}
.devicecontainer {
  height: 100%;
  margin: $space-l;
  display: flex;
  flex-direction: column;
}
.devicelist {
  flex-shrink: 2;
  overflow-y: scroll;
  height: calc(100% - #{$safespace});
  padding: $space-l $space-s $space-l $space-l;
  & > * + * {
    margin-top: $space-m;
  }
}
</style>

<docs>
  ### Is used to show registered devices in the device list panel in the form of ListChips.
  ```jsx
  <div style="width: 400px; height: 450px;">
    <ChipList :chips="[
    {name:'WereNoStrangersTo', id: 1},
    {name:'LoveYouKnowThe', id: 2},
    {name:'RulesAndSoDoI', id: 3},
    {name:'AFullCommitments', id: 40},
    {name:'WhatImThinkingOf', id: 555},
    {name:'YouWoildntGetThis', id: 6666},
    {name:'FromAnyOtherGuy', id: 1234},
    ]"/>
  </div>

  ```
</docs>
