<template lang="html">
	<tr v-on:click="showRoute" class="pointer">
		<td>{{route.id}}</td>
		<td>{{route.start}}</td>
	  	<td>{{route.end}}</td>
	  	<td>
	  		<button type="button" class="btn btn-xs remove" aria-label="Left Align" v-on:click.stop="deleteRoute(route.id)">
  				<span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
			</button>
	</td>
	</tr>
</template>

<script>
export default {
	props: ['route'],
	methods:{
		deleteRoute(){
			this.$emit('delRoute');
		},
		showRoute(){
  			this.$emit('showRoute');
		}
	}
}
</script>

<style lang="css">
.pointer{
	cursor: pointer;
}

.remove{
	color: red;
}
</style>