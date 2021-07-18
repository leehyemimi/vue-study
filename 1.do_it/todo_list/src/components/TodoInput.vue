<template>
  <div class="inputBox shadow">
	<input type="text" v-model="newTodoItem" placeholder="input" v-on:keyup.enter="addTodo">
	<span class="addContainer" v-on:click="addTodo">
	  <i class="addBtn fa fa-plus" aria-hidden="true"></i>
	</span>
	  
	<modal v-if="showModal" @close="showModal = false">
		<h3 slot="header">경고</h3>
		<div slot="footer" @click="showModal = false">
			할일을 입력하세요
			<i class="closeModalBtn fa fa-times" aria-hidden="true"></i>
		</div>
	</modal>
  </div>
</template>e

<script>
	import Modal from './common/Modal.vue'
	
	export default {
		data(){
			return{
				newTodoItem:'',
				showModal : false
			}
		},
		methods :{
			addTodo(){
				if(this.newTodoItem !== ''){
					var value = this.newTodoItem && this.newTodoItem.trim();
					//localStorage.setItem(value,value);
					this.$emit('addTodo',value)
					this.clearInput();
				}else{
					this.showModal = !this.showModal;
				}
			},
			clearInput(){
				this.newTodoItem = '';
			}
		},
		components :{
			Modal : Modal
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
