<template>
  <div class="root-layout"></div>
</template>

<script>
export default {
  name: "MemoModification",
  mounted: function() {},
  props: ["memo"],
  data() {
    return {};
  },
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.root-layout {
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: transparent;
}
</style>
