<template>
  <div class="root-layout" v-if="origin_memo">
    <div class="content-area">
      <textarea v-model="origin_memo.content"></textarea>
    </div>
    <div class="btn-area">
      <button v-on:click="cancel">취소</button>
      |
      <button v-on:click="complete">완료</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "MemoModification",
  mounted: function() {},
  props: ["memo"],
  data() {
    return {
      origin_memo: null
    };
  },
  mounted: function() {
    this.origin_memo = this.clone(this.memo);
  },
  methods: {
    cancel: function() {
      this.$emit("cancel");
    },
    complete: function() {
      if (this.origin_memo.content.length <= 0) {
        alert("내용을 입력해주세요.");
        return;
      }

      this.memo.content = this.origin_memo.content;
      this.$emit("complete");
    },
    clone: function(obj) {
      if (obj === null || typeof obj !== "object") return obj;

      var copy = obj.constructor();

      for (var attr in obj) {
        if (obj.hasOwnProperty(attr)) {
          copy[attr] = obj[attr];
        }
      }

      return copy;
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

.content-area {
  height: 85%;
}

.content-area textarea {
  display: block;
  position: relative;
  margin: auto;
  width: 95%;
  height: 100%;
  resize: none;
  background-color: transparent;
  outline: 0px;
  border: 0px;
}

.btn-area {
  text-align: right;
  display: block;
  position: relative;
}

.btn-area button {
  text-align: right;
  background-color: transparent;
  outline: 0px;
  border: 0px;
}
</style>
