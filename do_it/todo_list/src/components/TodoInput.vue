<template>
  <div class="inputBox shadow">
	<input type="text" v-model="newTodoItem" placeholder="input" v-on:keyup.enter="addTodo">
	<span class="addContainer" v-on:click="addTodo">
	  <i class="addBtn fa fa-plus" aria-hidden="true"></i>
	</span>
  </div>
</template>

<script>
export default {
  data(){
  	return{
		newTodoItem:''
	}
  },
  methods :{
	  addTodo(){
	  	if(this.newTodoItem !== ''){
	  		var value = this.newTodoItem && this.newTodoItem.trim();
	  		//localStorage.setItem(value,value);
			this.$emit('addTodo',value)
	  		this.clearInput();
		}
	  },
	  clearInput(){
	  	this.newTodoItem = '';
	  }
  }
}
</script>

<style lang="scss">
	input{
		outline: none;
	}
	.inputBox{
		background: #fff;
		height: 50px;
		line-height: 50px;
		border-radius: 5px;
		input{
			border-style: none;
			font-size: 0.9rem;
		}
	}
	.addContainer{
		float:right;
		background: linear-gradient(to right,#6478FB,#8763FB);
		display: inline-block;
		width: 3rem;
		border-radius: 0 5px 5px 0;
	}
	.addBtn {
		color: #ffffff;
		vertical-align: middle;
	}
</style>
