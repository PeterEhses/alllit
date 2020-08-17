<template>
  <Container type="card" class="devicecontainer">
    <Tag>Device List</Tag>
    <div class="devicelist">
      <ListChip />
      <ListChip />
      <ListChip />
    </div>
  </Container>
</template>

<script>
import ListChip from "@/templates/ListChip.vue"
import Container from "@/elements/Container/Container.vue"
import Tag from "@/elements/Tag.vue"
export default {
  name: "ChipList",
  status: "prototype",
  release: "0.0.1",
  components: {
    ListChip,
    Container,
  },
}
</script>

<style lang="scss" scoped>
.devicecontainer {
  margin: $space-l;
}
.devicelist {
  overflow-y: scroll;
  padding: $space-l;
  & > * + * {
    margin-top: $space-l;
  }
}
</style>

<docs>
  ```jsx
  <div style="width: 400px">
    <ChipList/>
  </div>

  ```
</docs>
