<template>
  <div style="display: flex">
    <div style="margin-top: 70px; margin-right: 20px">
      <scroll-sync vertical style="width: 50px; height: 500px">
        <div style="height: 2000px; width: 50px; background: black" />
      </scroll-sync>
    </div>
    <div>
      <scroll-sync
        horizontal
        style="height: 50px; width: 500px; margin-bottom: 20px"
      >
        <div style="height: 50px; width: 2000px; background: grey" />
      </scroll-sync>
      <scroll-sync vertical horizontal style="width: 500px; height: 500px">
        <div style="height: 2000px; width: 2000px; background: yellow" />
      </scroll-sync>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {}
  },
}
</script>

<style lang="scss" scoped></style>
