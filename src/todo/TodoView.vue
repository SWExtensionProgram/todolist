<template lang="html">
    <div class="root-layout">
		<div class="header-layout">
			<div class="header-title">{{title}}</div>
			<div v-if="viewType==='list'" class="header-selection">
				<button class="drop-btn">...<i class="fa fa-caret-down"></i>
				</button>
				<div class="drop-content">
					<button v-on:click="changeToTodoView()">생성</button>
				</div>
			</div>
		</div>
	<div class="content-layout">
		<div v-if="viewType==='revise'">
			<todoRevisePage :todo="todos[index]" :viewType="viewType" :index="index" @reviseTodo="reviseTodo" @canselTodo="canselTodo" />
		</div>		
		<div v-else-if="viewType==='todo'">
			<todoPage :todos="todos" @createTodo="createTodo" @canselTodo="canselTodo" />
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
import todoList from './TodoList';
import todoPage from './TodoPage';
import todoRevisePage from './TodoRevisePage';
import alert from './alert';
　
export default {
	name: 'TodoView',
    components: {
        todoPage,
        todoList,
		todoRevisePage,
		alert
    },
  data () {
    return {
		title:"TODO",
		viewType:'list',
		index:-1,
		isAlertVisible: false,
		todos: []
    }
  },
  	methods:{
		deleteTodo(i){
			this.changeToListView()
			this.todos.splice(i,1);
		},
		createTodo(name,content,deadline){
			if(content == null){
				this.showAlert();
			}
		
			else{
				if(name==null)name="No Title";
				if(deadline==null)deadline=this.getTodayDate();
				this.changeToListView()
				this.todos.push({name:name, content:content, deadline:deadline});
				this.name = null
				this.content = null
				this.deadline = null
				this.sortDeadline()

			}
		},
		reviseTodo(name,content,deadline,i){
			this.todos.splice(i,1);
			if(content ==null){
				this.showAlert();
			}
			else{	
				this.changeToListView()
				this.todos.push({name:name, content:content, deadline:deadline});
				this.name = null
				this.content = null
				this.deadline = null
				this.sortDeadline()
			}
		},
		canselTodo(){
			this.changeToListView();
		},
		changeToListView(){
			this.viewType = 'list'
		},
		changeToTodoView(){
			this.viewType = 'todo'
		},
		changeToReviseView(i){
			this.index = i;
			this.viewType = 'revise'
			console.log("index")
			console.log(this.index)
		},
		showAlert(){
			this.isAlertVisible = true;
		},
		closeAlert(){
			this.isAlertVisible = false;
		},
		getTodayDate(){
			var date = new Date();
			var year = date.getFullYear(); 
			var month = new String(date.getMonth()+1); 
			var day = new String(date.getDate()); 

			// 한자리수일 경우 0을 채워준다. 
			if(month.length == 1){ 
			  month = "0" + month; 
			} 
			if(day.length == 1){ 
			  day = "0" + day; 
			} 

			return (year + "-" + month + "-" + day);
			
		}
	},
	computed:{
		sortDeadline(){
			this.todos.sort(function(a,b){
				return new Date(a.deadline) - new Date(b.deadline);
			}
		);
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
  position: relative;
  height: calc(100% - 40px);
}


</style>