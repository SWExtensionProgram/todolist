<template lang="html">
    <div class="root-layout">
		<div class="header-layout">
			<div class="header-title">{{title}}</div>
			<div v-if="viewType==='list'" class="header-selection">
				<button class="drop-btn">...
				<i class="fa fa-caret-down"></i>
				</button>
				<div class="drop-content">
					<button v-on:click="changeToTodoView()">생성</button>
				</div>
			</div>
		</div>
	<div class="content-layout">
		<div v-if="viewType==='revise'">
			<todoRevisePage :todo="todos[index]" :index="index" @reviseTodo="reviseTodo" @canselTodo="canselTodo" />
			<alert v-show="isAlertVisible" @close="closeAlert"/>
		</div>		
		<div v-else-if="viewType==='todo'">
			<todoPage @createTodo="createTodo" @canselTodo="canselTodo" />
			<alert v-show="isAlertVisible" @close="closeAlert"/>
		</div>
		<div v-else>
				<todoList :todos="todos" @deleteTodo="deleteTodo"
				@sortDeadline="sortDeadline" @changeToReviseView="changeToReviseView"/>
	
		</div>
	</div>
	</div>
</template>
　
<script>
import todoList from "./TodoList";
import todoPage from "./TodoPage";
import todoRevisePage from "./TodoRevisePage";
import alert from "./TodoAlert";
export default {
  name: "TodoView",
  components: {
    todoPage,
    todoList,
    todoRevisePage,
    alert
  },
  data() {
    return {
      title: "TODO",
      viewType: "list",
      index: -1,
      isAlertVisible: false,
      todos: [],
      last_index: -1
    };
  },
  mounted: function() {
    var key_list;
    let self = this;
    chrome.storage.local.get(["todokeys"], function(key_list) {
      if (!key_list.todokeys) {
      } else {
        let new_keys = key_list.todokeys;
        console.log(new_keys);
        new_keys.forEach((key, index) => {
          chrome.storage.local.get([key], function(result) {
            self.todos.push(result[key]);
            self.last_index = result[key].id;
          });
        });
      }
    });
  },
  methods: {
    deleteTodo(i) {
      this.changeToListView();
      let self = this;
      var current_id = this.todos[i].id.toString();
      chrome.storage.local.remove(["t" + current_id], function() {
        var key_list;
        chrome.storage.local.get(["todokeys"], function(key_list) {
          var new_key_list = key_list.todokeys;
          new_key_list.splice(new_key_list.indexOf("t" + current_id), 1);
          chrome.storage.local.set({ todokeys: new_key_list });
          self.todos.splice(i, 1);
        });
      });
    },
    createTodo(name, content, deadline) {
      var todokeysArr;
      let self = this;
      if (content == null || content == "") {
        this.showAlert();
      } else {
        if (name == null || name == "") name = "No Title";
        if (deadline == null) deadline = this.getTodayDate();
        this.changeToListView();
        this.last_index = this.last_index + 1;
        this.todos.push({
          id: this.last_index,
          name: name,
          content: content,
          deadline: deadline
        });
        var current_index = this.todos.length - 1;
        var new_key = "t" + this.todos[current_index].id.toString();
        console.log(this.todos);

        var todokey_list;
        var todokey_list2;

        chrome.storage.local.get(["todokeys"], function(todokey_list) {
          console.log(todokey_list.todokeys);
          if (!todokey_list.todokeys) {
            chrome.storage.local.set({
              todokeys: ["t" + self.todos[current_index].id.toString()]
            });
          } else {
            todokey_list2 = todokey_list.todokeys.slice();
            todokey_list2.push("t" + self.todos[current_index].id.toString());

            chrome.storage.local.set({ todokeys: todokey_list2 }, function() {
              var wang;
              chrome.storage.local.get(["todokeys"], function(wang) {
                console.log("changed value", wang.todokeys);
              });
            });
          }
          var new_key = "t" + self.todos[current_index].id.toString();
          chrome.storage.local.set({ [new_key]: self.todos[current_index] });
        });

        this.name = null;
        this.content = null;
        this.deadline = null;
      }
    },
    reviseTodo(name, content, deadline, i) {
      console.log("content");
      console.log(content);
      if (content == "") {
        this.showAlert();
      } else {
        var current_id = this.todos[i].id;
        this.todos.splice(i, 1);
        this.changeToListView();
        this.todos.push({
          id: current_id,
          name: name,
          content: content,
          deadline: deadline
        });
        //
        var current_index = this.todos.length - 1;
        chrome.storage.local.set({
          ["t" + current_id]: this.todos[current_index]
        });

        this.name = null;
        this.content = null;
        this.deadline = null;
      }
    },
    canselTodo() {
      this.changeToListView();
    },
    changeToListView() {
      this.viewType = "list";
      this.title = "TODO";
    },
    changeToTodoView() {
      this.viewType = "todo";
      this.title = "NEW TODO";
    },
    changeToReviseView(i) {
      this.index = i;
      this.viewType = "revise";
      console.log("index");
      console.log(this.index);
      this.title = "CHANGE TODO";
    },
    showAlert() {
      this.isAlertVisible = true;
    },
    closeAlert() {
      this.isAlertVisible = false;
    },
    getTodayDate() {
      var date = new Date();
      var year = date.getFullYear();
      var month = new String(date.getMonth() + 1);
      var day = new String(date.getDate());

      // 한자리수일 경우 0을 채워준다.
      if (month.length == 1) {
        month = "0" + month;
      }
      if (day.length == 1) {
        day = "0" + day;
      }

      return year + "-" + month + "-" + day;
    }
  },
  computed: {
    todoesortDeadline() {
      this.todos.sort(function(a, b) {
        return new Date(a.deadline) - new Date(b.deadline);
      });
    }
  }
};
</script>

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
  padding: 12px 0px;
  display: block;
  text-align: left;
  outline: 0px;
  border: 0px;
}
.drop-content a {
  float: none;
  color: black;
  padding: 12px 0px;
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
  position: relative;
  height: calc(100% - 40px);
}
</style>