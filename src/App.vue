<template>
  <div class="root-layout" style="background-image: url('/static/image/nature-3294543_1920.jpg')">
    <div class="timezone-selection">
      <select onfocus="this.size=6;" onchange="this.size=1; this.blur();" v-model="timezone">
        <!-- <select size="5" v-model="timezone"> -->
        <option value selected disabled hidden>Timezone</option>
        <option v-for="(zone, index) in timezone_list" :key="index">{{zone}}</option>
      </select>
    </div>
    <div class="time-show-layout">
      <div class="time-wrapper">
        <div class="date">{{ date }}</div>
        <br>
        <br>
        <br>
        <br>
        <div class="time">{{ time }}</div>
      </div>
    </div>
    <div class="list-show-layout">
      <button v-on:click="change_show_state(0)">memo</button>
      <span class="list-show-bar">|</span>
      <button v-on:click="change_show_state(1)">todo</button>
    </div>
    <div v-if="todoShow" class="list-layout" style="right: calc(3% + 121px);">
      <todo></todo>
    </div>
    <div v-if="memoShow" class="list-layout">
      <memo></memo>
    </div>
  </div>
</template>

<script>
var moment = require("moment-timezone");
import memo from "./memo/MemoLayout.vue";
import todo from "./todo/TodolistView.vue";
export default {
  name: "time",
  data() {
    return {
      time: "",
      date: "",
      timezone: "Asia/Seoul",
      timezone_list: null,
      todoShow: false,
      memoShow: false
    };
  },
  components: {
    memo,
    todo
  },
  beforeCreate: function() {},
  mounted: function() {
    this.show_time();
    // 기존에 설정된 timezone이 있으면 불러와야 하는 부분
    var stored_timezone;
    let self = this;
    // storage에서 불러오는 부분
    chrome.storage.local.get(["timezone"], function(stored_timezone) {
      if (!stored_timezone.timezone) {
        self.timezone = "Asia/Seoul";
      } else {
        self.timezone = stored_timezone.timezone;
      }
    });

    moment.tz.setDefault(this.timezone);
    this.timezone_list = moment.tz.names();
  },
  watch: {
    timezone: function() {
      // timezone 변경해야 하는 부분. XXX = this.timezone의 형태로 변경 가능.
      this.show_time();
      let self = this;
      chrome.storage.local.set({ timezone: this.timezone }, function() {});
    }
  },
  methods: {
    show_time: function() {
      var timerID = setInterval(this.update_time, 1000);
      this.update_time();
    },
    update_time: function() {
      this.date = moment()
        .tz(this.timezone)
        .format("YYYY[.]MM[.]DD[.]");
      this.time = moment()
        .tz(this.timezone)
        .format("HH:mm:ssa");
    },
    change_show_state: function(obj) {
      if (obj == 0) {
        this.memoShow = !this.memoShow;
        this.todoShow = false;
      } else if (obj == 1) {
        this.memoShow = false;
        this.todoShow = !this.todoShow;
      } else {
        this.memoShow = false;
        this.todoShow = false;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.root-layout {
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: transparent;
  overflow-y: hidden;
  background-size: 100%;
  background-repeat: no-repeat;
  background-position: center;
}

.timezone-selection {
  display: block;
  position: relative;
  width: 100%;
  height: 10%;
}

.timezone-selection select {
  outline: 0px;
  font-size: 100%;
  position: absolute;
  float: right;
  width: max-content;
  text-align-last: right;
  border: 0px;
  outline: 0px;
  background-color: transparent;
  overflow: scroll;
  font-size: 25px;
  top: 25%;
  z-index: 1;
  right: 1%;
}

::-webkit-scrollbar {
  display: none !important;
}

.time-show-layout {
  position: relative;
  height: 80%;
}

.time-show-layout .time-wrapper {
  position: relative;
  display: inline-block;
  top: 30%;
  left: calc(50% - 250px);
}

.time-show-layout .date {
  /* font-size: 150px; */
  display: block;
  position: relative;
  float: left;
  font-size: 56px;
  /* font-weight: bold; */
}

.time-show-layout .time {
  /* font-size: 200px; */
  display: block;
  position: relative;
  float: left;
  font-size: 86px;
  font-weight: bold;
  top: -15px;
}

.list-show-layout {
  position: relative;
  height: 10%;
  font-size: 100%;
  padding-right: 3%;
}

.list-show-layout button {
  border: 0px;
  outline: 0px;
  float: right;
  font-size: 30px;
  font-weight: bold;
  z-index: 2;
}

.list-show-bar {
  float: right;
  font-size: 35px;
}

.list-layout {
  /* background-image: linear-gradient(
    to right,
    red,
    orange,
    yellow,
    violet,
    yellow,
    orange,
    red
  ); */
  background-color: #ccc;
  position: absolute;
  width: 280px;
  height: 200px;
  bottom: calc(55px + 5%);
  right: calc(3% + 23px);
  border-radius: 15px;
  z-index: 3;
}

.list-layout:after {
  content: "";
  position: absolute;
  border-top: 30px solid #ccc;
  border-right: 15px solid transparent;
  border-left: 15px solid transparent;
  bottom: -26px;
  right: 15px;
  z-index: 1;
}
</style>
