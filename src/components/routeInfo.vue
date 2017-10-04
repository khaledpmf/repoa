<template lang="html">
		<div id="route-info">
			<p class="info distance"><span class="glyphicon glyphicon-road" aria-hidden="true"></span>&nbsp;&nbsp;&nbsp;{{distance}}</p>
			<p class="info duration"><span class="glyphicon glyphicon-time" aria-hidden="true"></span>&nbsp;&nbsp;&nbsp;{{duration}}</p>
		</div>
</template>

<script>
export default {
	name: 'routeinfo',
	props: ['distance', 'duration']
}
</script>

<style lang="css">
#route-info{
	background: #337ab7;
	margin-bottom: 3px;
}

.info{
	display: inline;
	color: white;
	margin: 2px 30px;
}
</style>	