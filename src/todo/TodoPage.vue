<template>
<div class="root-layout">
	<div class="content-area">
		<input class="name-area" type="text" placeholder="제목을 입력하세요" v-model="name">

		<textarea 
 			placeholder="내용을 입력하세요" 
			v-model="content"></textarea>
		<input class="date-area" type="date" v-model="deadline">
	</div>
		<div class="btn-area">
			<button v-on:click="canselTodo()">취소</button>
			<button v-on:click="createTodo(name,content,deadline)">완료</button>
		</div>

</div>
</template>

<script>

export default {
  name: 'TodoPage',

  	methods:{
		createTodo(name,content,deadline){
			this.$emit('createTodo',name,content,deadline);
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
  vertical-text-align: middle;
  font-family: Arial;
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
    font-family: Arial;
}

.date-area{
	background-color: transparent;
	outline: 0px;
	border: 0px;
	padding: 0px 15px;
	font-family: Arial;
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
