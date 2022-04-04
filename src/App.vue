<template>
	<div id="app">
		<simon-game />
	</div>
</template>

<script>
import SimonGame from './components/SimonGame.vue';
	export default {
		name: 'App',
		components: {
			SimonGame,
		},
	};
</script>

<style>
	* {
		box-sizing: border-box;
	}
	html {
		font-size: 16px;
		font-family: "Helvetica Neue", Helvetica, Roboto, Arial, sans-serif;
	}
	body {
		padding: 0;
		margin: 0;
	}
	:root {
		--red-color: rgba(255, 0, 0, 0.65);
		--red-color-active: red;
		--red-color-hover: rgba(255, 0, 0, 0.9);
		--blue-color: rgba(0, 0, 255, 0.65);
		--blue-color-active: blue;
		--blue-color-hover: rgba(0, 0, 255, 0.9);
		--yellow-color: rgba(255, 255, 0, 0.5);
		--yellow-color-active: yellow;
		--yellow-color-hover: rgba(255, 255, 0, 0.9);
		--green-color: rgba(0, 128, 0, 0.65);
		--green-color-active: green;
		--green-color-hover: rgba(0, 128, 0, 0.9);
	}
	#app {
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
		padding: 30px;
	}
</style>