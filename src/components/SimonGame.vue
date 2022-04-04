<template>
	<div class="game">
		<section class="gameboard">
		<ul class="gameboard__buttons">
			<li
				v-for="(item, index) in layout"
				:key="index"
				class="gameboard__button">
				<SimonButton
					:index="index"
					:color="item.color"
					:name="item.name"
					ref="button"
					@click="handleButtonClick"
				/>
			</li>
		</ul>
		</section>
		<aside class="sidebar">
		<section class="play">
			<h1 class="play__round">Round: {{ round }} </h1>
			<button class="play__button" @click="init">Start</button>
			<span v-show="gameover" class="play__result">Sorry, you lost in round <strong>{{ round }}</strong>!</span>
		</section>

		<section class="difficulty">
			<h2 class="difficulty__heading">Difficulty</h2>
			<ul class="difficulty__list">
			<li
				v-for="(item, index) in difficultyLevels"
				:key="index"
				class="difficulty__item">
				<input
				v-model="difficulty"
				:value="item.value"
				:id="`difficulty-${item.value}`"
				type="radio"
				class="difficulty__input"
				/>
				<label :for="`difficulty-${item.value}`">{{ item.text }}</label>
			</li>
			</ul>
		</section>
		</aside>
	</div>
</template>

<script>
	import SimonButton from '@/components/SimonButton';
	import { layout, difficultyLevels } from '@/shared/constants';
	import { sleep } from '@/shared/functions';
	export default {
		name: 'SimonTheGame',
		components: {
			SimonButton,
		},
		data: () => ({
			layout,
			difficultyLevels,
			round: 0,
			current: 0,
			playSequence: [],
			prevButtonindex: null,
			difficulty: 'easy',
			gameover: false,
		}),
		computed: {
			interval() {
				return this.difficultyLevels[this.difficulty].interval;
			},
		},
		methods: {
			init() {
				this.reset();
				this.generateNewRound();
			},
			reset() {
				this.round = 0;
				this.gameover = false;
				this.playSequence = [];
				this.prevButtonindex = null;
			},
			async generateNewRound() {
				this.round += 1;
				this.current = 0;
				const index = this.generateNextButtonindex();
				this.prevButtonindex = index;
				this.playSequence.push(index);
				for (const index of this.playSequence) {
					this.$refs.button[index].play();
					await sleep(this.interval);
				}
			},
			generateNextButtonindex() {
				let num;
				do {
					num = Math.round(Math.random() * 3);
				} while (num === this.prevButtonindex);
				return num;
			},
			handleButtonClick(index) {
				if (!this.round) return;
				const currentindex = this.playSequence[this.current];
				if (index === currentindex) {
					this.incrementCurrent();
				} else {
					this.gameover = true;
					this.playSequence = [];
				}
			},
			async incrementCurrent() {
				if (this.current !== this.round - 1) {
					this.current += 1;
				} else {
					await sleep(1200);
					this.generateNewRound();
				}
			},
		},
	};
</script>

<style scoped>
	.game {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	@media screen and (min-width: 1224px) {
		.game {
			flex-direction: row;
		}
	}
	@media screen and (min-width: 1224px) {
		.gameboard {
			margin-right: 3rem;
		}
	}
	.gameboard__buttons {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr 1fr;
		padding: 0;
		margin: 0;
		height: 75vw;
		width: 75vw;
		max-height: 500px;
		max-width: 500px;
		border-radius: 50%;
		box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.5);
	}
	@media screen and (min-width: 1224px) {
		.gameboard {
			margin-right: 3rem;
		}
	}
	.gameboard__button {
		display: block;
		height: 100%;
		width: 100%;
		overflow: hidden;
		border: 10px solid white;
	}
	.gameboard__button:first-of-type {
		border-radius: 250px 0 0 0;
		border-right-width: 5px;
		border-bottom-width: 5px;
	}
	.gameboard__button:nth-of-type(2) {
		border-radius: 0 250px 0 0;
		border-left-width: 5px;
		border-bottom-width: 5px;
	}
	.gameboard__button:nth-of-type(3) {
		border-radius: 0 0 0 250px;
		border-right-width: 5px;
		border-top-width: 5px;
	}
	.gameboard__button:last-of-type {
		border-radius: 0 0 250px 0;
		border-top-width: 5px;
		border-left-width: 5px;
	}
	.sidebar {
		padding: 30px;
		min-width: 270px;
	}
	.play {
		margin-bottom: 2rem;
	}
	.play__round,
	.difficulty__heading {
		display: block;
		margin: 0;
		margin-bottom: 0.5rem;
		color: black;
		font-size: 2rem;
	}
	.difficulty__list {
		margin: 0;
		padding: 0;
		list-style-type: none;
	}
	.difficulty__item {
		font-size: 1rem;
		line-height: 1.5rem;
	}
	.play__button {
		display: block;
		padding: 0.5rem 1rem;
		min-width: 100px;
		font-size: 1rem;
	}
	.play__result {
		display: block;
		margin-top: 1rem;
	}
	.play,
	.difficulty {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	@media screen and (min-width: 1224px) {
		.play,
		.difficulty {
			display: block;
		}
	}
</style>