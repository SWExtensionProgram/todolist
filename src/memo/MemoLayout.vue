<template>
  <div class="root-layout">
    <div class="header-layout">
      <div class="header-title">{{ title }}</div>
      <div class="header-selection">
        <button class="drop-btn">
          menu
          <i class="fa fa-caret-down"></i>
        </button>
        <div class="drop-content">
          <!-- <button v-on:click="create_item()" v-if="mode != 2">생성</button> -->
          <!-- <button v-on:click="delete_item()" v-if="mode != 1">수정</button> -->
        </div>
      </div>
    </div>
    <div class="content-layout">
      <memo-detail :memo="memo_list[selected_index]" v-if="mode == 1"></memo-detail>
      <memo-modification :memo="memo_list[selected_index]" v-else-if="mode == 2"></memo-modification>
      <memo-list
        :memo_list="memo_list"
        v-on:delete="delete_item"
        v-on:modification="go_modification"
        v-else
      ></memo-list>
    </div>
    <!-- <router-view class="content-layout"></router-view> -->
  </div>
</template>

<script>
import memoDetail from "./MemoDetail.vue";
import memoList from "./MemoList.vue";
import memoModification from "./MemoModification.vue";
export default {
  name: "Memo",
  components: {
    memoDetail,
    memoList,
    memoModification
  },
  watch: {
    mode: function() {
      if (this.mode == 1) {
        this.title = this.memo_list[this.selected_index];
      }
    }
  },
  mounted: function() {
    // memo list 조회해오는 코드 필요
    // this.memo_list = XXX 형식
  },
  data() {
    return {
      title: "MEMO",
      mode: 0,
      selected_index: -1,
      memo_list: [
        { id: 1, content: "hello" },
        { id: 2, content: "world" },
        { id: 2, content: "worldhellowdsfjxzkljksjflsjflksxkxk" }
      ]
    };
  },
  methods: {
    create_item: function() {
      this.mode = 1;
    },
    delete_item: function(index) {
      this.mode = 3;
      this.memo_list.splice(index, 1);
    },
    go_modification: function(index) {
      this.mode = 2;
      this.selected_index = index;
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
}

.header-layout {
  display: block;
}

.header-title {
  font-size: 23px;
  font-weight: bold;
  text-align: left;
  display: inline-block;
  left: 3%;
  width: fit-content;
  position: relative;
}

.header-selection {
  display: inline-block;
  float: right;
  height: 23px;
}

.header-selection .drop-btn {
  display: inline-block;
  background-color: transparent;
  outline: 0px;
  border: 0px;
  top: 1%;
}

.drop-content {
  display: none;
  position: absolute;
  background-color: transparent;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

.drop-content button {
  background-color: transparent;
  min-width: 160px;
  padding: 12px 16px;
  display: block;
  text-align: left;
  outline: 0px;
  border: 0px;
}

.drop-content a {
  float: none;
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: left;
}

.drop-content a:hover {
  background-color: #ddd;
}

.header-selection:hover .drop-content {
  display: block;
}

.content-layout {
  overflow-y: scroll;
  position: relative;
  height: calc(100% - 40px);
}
</style>
