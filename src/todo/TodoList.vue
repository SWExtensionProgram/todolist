<template>
<div class="root-layout">
	<div class="content-layout" v-if="todos.length">
		<div class="content-list">
			<div class="content-item" v-for="(todo, index) in todos">
				<button class="content-btn" v-on:click="changeToReviseView(index)">
					<div class="content-content">{{ todo.name }}</div>
				</button>
				<i class="content-icon" v-on:click="deleteTodo(index)">x</i>
			</div>
		</div>
	</div>
	<div class="non-content-layout" v-else>
		<div class="non-content-content">항목이 없습니다.</div>
	</div>
</div>
</template>

<script>
export default {
  name: 'TodoList',
  props : ['todos'],
  methods:{
	deleteTodo(index){
		this.$emit('deleteTodo',index);
	},
	changeToReviseView(index){
		this.$emit('changeToReviseView',index);
	}
	},
	computed:{

	sortDeadline(){
		this.$emit('sortDeadline');
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
  overflow-y: scroll;
}
.content-list {
  padding-left: 10px;
  padding-right: 10px;
}
.content-btn {
  position: relative;
  width: 93%;
  background-color: transparent;
  outline: 0px;
  border: 0px;
}
.content-content {
  display: inline-block;
  width: 100%;
  padding: 0 5px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: left;
}
.content-icon {
  display: inline-block;
  position: relative;
  width: fit-content;
  float: right;
}
.non-content-layout {
  display: table;
  height: 100%;
  width: 100%;
}
.non-content-content {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
  overflow-y: hidden;
}
</style>
