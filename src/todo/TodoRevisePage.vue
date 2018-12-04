<template>
<div class="root-layout">
	<div class="input-group" style="margin-bottom:10px;">
		<input type="text" class="form-control" 
			placeholder="할일을 입력하세요" 
			v-model="todo.name">
		<textarea class="form-control" 
 			placeholder="내용을 입력하세요" 
			v-model="todo.content"></textarea>
		<input type="date" v-model="todo.deadline">

		<div class="btn-area">
			<button v-on:click="canselTodo()">취소</button>
			<button v-on:click="reviseTodo(todo.name,todo.content,todo.deadline,index)">완료</button>
		</div>
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
  background-image: linear-gradient(salmon, pink);
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
