<template>
  <div class="root-layout">
    <div class="header-layout">
      <div class="header-title">{{ title }}</div>
      <div class="header-selection">
        <button class="drop-btn">
          ...
          <i class="fa fa-caret-down"></i>
        </button>
        <div class="drop-content">
          <button v-on:click="create_mode()">생성</button>
          <!-- <button v-on:click="delete_item()" v-if="mode != 1">수정</button> -->
        </div>
      </div>
    </div>
    <div class="content-layout">
      <memo-detail :memo="memo_list[selected_index]" v-if="show_mode == 1"></memo-detail>
      <memo-modification
        :memo="memo_list[selected_index]"
        v-on:cancel="modification_cancel"
        v-on:complete="modification_complete"
        v-else-if="show_mode == 2"
      ></memo-modification>
      <memo-creation
        v-on:complete="create_complete"
        v-on:cancel="create_cancel"
        v-else-if="show_mode == 4"
      ></memo-creation>
      <memo-list
        :memo_list="memo_list"
        v-on:delete="delete_item"
        v-on:modification="modification_mode"
        v-else
      ></memo-list>
      <!-- <memo-creation v-on:creation="create_item"></memo-creation> -->
    </div>
    <!-- <router-view class="content-layout"></router-view> -->
  </div>
</template>

<script>
import memoDetail from "./MemoDetail.vue";
import memoList from "./MemoList.vue";
import memoModification from "./MemoModification.vue";
import memoCreation from "./MemoCreation.vue";
export default {
  name: "Memo",
  components: {
    memoDetail,
    memoList,
    memoModification,
    memoCreation
  },
  watch: {
    mode: function() {
      if (this.mode == 1 || this.mode == 2) {
        this.title = this.memo_list[this.selected_index].content;
      }
    }
  },
  computed: {
    show_mode: function() {
      console.log("show_mode :", this.mode);
      return this.mode;
    }
  },
  mounted: function() {
    // memo list 조회해오는 코드 필요
    // this.memo_list = XXX 형식
    this.mode = 3;
    this.last_index = this.memo_list[-1].id;
  },
  data() {
    return {
      title: "MEMO",
      mode: 0,
      selected_index: -1,
      last_index: -1,
      memo_list: [
        { id: 1, content: "hello" },
        { id: 2, content: "world" },
        { id: 3, content: "worldhellowdsfjxzkljksjflsjflksxkxk" }
      ]
    };
  },
  methods: {
    modification_cancel: function() {
      this.change_title_origin();
    },
    modification_complete: function() {
      this.change_title_origin();
      // this.memo_list.splice(this.selected_index, 1, new_object);
    },
    create_cancel: function() {
      this.change_title_origin();
    },
    create_complete: function(new_memo) {
      this.change_title_origin();
      new_memo.id = this.last_index + 1;
      this.last_index = this.last_index + 1;
      console.log(new_memo);
      this.memo_list.splice(0, 0, new_memo);
    },
    create_mode: function() {
      this.mode = 4;
      this.title = "NEW MEMO";
    },
    change_title_origin: function() {
      this.mode = 3;
      this.title = "MEMO";
    },
    delete_item: function(index) {
      this.memo_list.splice(index, 1);
      // console.log(this.memo_list, this.mode);
      this.change_title_origin();
    },
    modification_mode: function(index) {
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
  width: 81%;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
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
  width: 50px;
  text-align: right;
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
