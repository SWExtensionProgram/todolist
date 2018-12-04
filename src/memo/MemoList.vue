<template>
  <div class="root-layout">
    <div class="content-layout" v-if="memo_list.length">
      <div class="content-list">
        <div class="content-item" v-for="(memo, index) in memo_list" :key="index">
          <button class="content-btn" v-on:click="go_modification(index)">
            <div class="content-content">{{ memo.content }}</div>
          </button>
          <i class="content-icon" v-on:click="delete_item(index)">x</i>
        </div>
      </div>
    </div>
    <div class="non-content-layout" v-else>
      <div class="non-content-content">항목이 없습니다.</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MemoList",
  props: ["memo_list"],
  data() {
    return {};
  },
  mounted: function() {
    // console.log("list :", memo_list);
  },
  methods: {
    go_detail: function(index) {},
    go_modification: function(index) {
      this.$emit("modification", index);
    },
    delete_item: function(index) {
      this.$emit("delete", index);
    }
  }
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
  overflow-y: scroll;
}

.content-list {
  padding-left: 10px;
  padding-right: 10px;
}

.content-btn {
  position: relative;
  width: 93%;
  background-color: transparent;
  outline: 0px;
  border: 0px;
}

.content-content {
  display: inline-block;
  width: 100%;
  padding: 0 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: left;
}

.content-icon {
  display: inline-block;
  position: relative;
  width: fit-content;
  float: right;
}

.non-content-layout {
  display: table;
  height: 100%;
  width: 100%;
}

.non-content-content {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
  overflow-y: hidden;
}
</style>
