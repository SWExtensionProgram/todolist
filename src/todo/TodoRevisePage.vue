<template>
<div class="root-layout">
	<div class="content-area">
		<input class="name-area" type="text" placeholder="제목을 입력하세요" v-model="todo.name">
		<textarea
			v-model="todo.content"></textarea>
		<input class="date-area" type="date" v-model="todo.deadline">
	</div>
		<div class="btn-area">
			<button v-on:click="canselTodo()">취소</button>
			<button v-on:click="reviseTodo(todo.name,todo.content,todo.deadline,index)">완료</button>
		</div>
</div>

</template>

<script>

export default {
  name: 'TodoReviseView',
  props : ['todo','viewType','index'],

  	methods:{
		reviseTodo(name,content,deadline,i){
			this.$emit('reviseTodo',name,content,deadline,i);
		},
		canselTodo(){
			this.$emit('canselTodo');
		}
	}
}
</script>

<style scoped>
.root-layout {
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: transparent;
}
</style>


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
  height: 70%;
  resize: none;
  background-color: transparent;
  outline: 0px;
  border: 0px;
  border-bottom: 1px solid #eeeeee;
  padding: 0 15px;
  font-size: 13px;
  font-weight: bold;
  vertical-text-align: middle;
}
.name-area{
	width:95%;
	resize:none;  
	background-color: transparent;
	outline: 0px;  
	border: 0px; 
	padding: 5px 15px; 
	border-bottom: 1px solid #eeeeee;  
	font-size: 13px;
	font-weight: bold;
}

.date-area{
	background-color: transparent;
	outline: 0px;
	border: 0px;
	padding: 0px 15px
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