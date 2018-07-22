<template>
  <div class="clearAll">
    <span class="clearAllBtn" @click="clearTodo">
		clearAll
	</span>
  </div>
</template>

<script>
export default {
	methods:{
		clearTodo(){
			this.$emit('removeAll')
		}
	}
}
</script>

<style lang="scss">
  .clearAll{
    width: 8.5rem;
	  height: 50px;
	  line-height: 50px;
	  background: #fff;
	  border-radius: 5px;
	  margin:0 auto
  }
	.clearAllBtn{
		color:#e20303;
		display: block;
	}
</style>
