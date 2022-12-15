<template>
	<div class="info-box">
		<v-icon size="80">
			{{icon}}
		</v-icon>
		<h4>{{title}}</h4>
		<text>{{message}}</text>
	</div>
</template>

<script>
	export default {
		props: ["title","icon","message"]
	}
</script>

<style lang="scss" scoped>
.info-box {
	width: 200px;
	height: 200px;
	margin: 5px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

text {
	font-size: 12px;
	padding: 1px;
}
</style>