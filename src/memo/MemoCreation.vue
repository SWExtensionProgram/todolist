<template>
  <div class="root-layout">
    <div class="content-area">
      <textarea v-model="memo.content"></textarea>
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
  data() {
    return {
      memo: {
        id: null,
        content: ""
      }
    };
  },
  methods: {
    cancel: function() {
      this.$emit("cancel");
    },
    complete: function() {
      if (this.memo.content.length <= 0) {
        alert("내용을 입력해주세요.");
        return;
      }

      this.memo = this.memo;
      this.$emit("complete", this.memo);
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
