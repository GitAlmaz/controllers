<template>
	<div id="app">
		<div class="container">
			<Controller
				ref="ctrl1"
				:extra="{
					text: 'Сумма',
					method: () => (ctrl1 = ctrl2 + ctrl3)
				}"
				:tab="() => ({ next: this.$refs.ctrl2, prev: this.$refs.ctrl3 })"
				:propNum="ctrl1"
			/>
			<Controller
				ref="ctrl2"
				:extra="{
					text: 'Константа',
					method: () => setVal(['ctrl2', 'ctrl3'], 1000)
				}"
				:tab="() => ({ next: this.$refs.ctrl3, prev: this.$refs.ctrl1 })"
				:propNum="ctrl2"
				@getVal="val => setVal(['ctrl2', 'ctrl3'], val)"
			/>
			<Controller
				ref="ctrl3"
				:tab="() => ({ next: this.$refs.ctrl1, prev: this.$refs.ctrl2 })"
				:propNum="ctrl3"
				@getVal="val => setVal(['ctrl2', 'ctrl3'], val)"
			/>
		</div>
	</div>
</template>

<script>
	import Controller from './components/Controller.vue'

	export default {
		name: 'App',
		components: {
			Controller
		},
		data: () => ({
			ctrl1: 0,
			ctrl2: 0,
			ctrl3: 0
		}),
		methods: {
			setVal(to, val) {
				to.map(it => (this[it] = +val))
			}
		},
		mounted() {}
	}
</script>

<style lang="scss">
	* {
		margin: 0;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #f5f5f5;
		background-color: #333;
	}
	.container {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
</style>
