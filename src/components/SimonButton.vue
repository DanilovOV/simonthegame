<template>
  <button :class="buttonClasses" @click="handleClick" ref="button" />
</template>

<script>
	export default {
		name: 'SimonButton',
		props: {
			index: {
				type: Number,
				required: true,
			},
			color: {
				type: String,
				required: true,
			},
			name: {
				type: String,
				required: true,
			},
		},
		data: () => ({
			sound: null,
		}),
		created() {
			const path = require(`@/assets/Sounds/${this.name}`);
			this.sound = new Audio(path);
		},
		computed: {
			buttonClasses() {
				return ['simon-button', this.color];
			},
		},
		methods: {
			play() {
				this.sound.play();
				this.$refs.button.classList.add('active');
				setTimeout(() => {
					this.$refs.button.classList.remove('active');
				}, 400);
			},
			handleClick() {
				this.play();
				this.$emit('click', this.index);
			},
		},
	};
</script>

<style scoped>
	.simon-button {
		width: inherit;
		height: inherit;
		border: none;
	}
	.simon-button:focus {
		box-shadow: inset 0px 0px 50px 0 rgba(0, 0, 0, 0.25);
		outline: none;
	}
	.simon-button.blue {
		background-color: var(--blue-color);
	}
	.simon-button.blue.active {
		background-color: var(--blue-color-active);
	}
	.simon-button.blue:hover {
		background-color: var(--blue-color-hover);
	}
	.simon-button.red {
		background-color: var(--red-color);
	}
	.simon-button.red.active {
		background-color: var(--red-color-active);
	}
	.simon-button.red:hover {
		background-color: var(--red-color-hover);
	}
	.simon-button.yellow {
		background-color: var(--yellow-color);
	}
	.simon-button.yellow.active {
		background-color: var(--yellow-color-active);
	}
	.simon-button.yellow:hover {
		background-color: var(--yellow-color-hover);
	}
	.simon-button.green {
		background-color: var(--green-color);
	}
	.simon-button.green.active {
		background-color: var(--green-color-active);
	}
	.simon-button.green:hover {
		background-color: var(--green-color-hover);
	}
</style>