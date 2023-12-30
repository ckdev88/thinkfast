<template>
	<h1 :disabled="isPlaying || !isFirstTime">Reaction timer, <em>BAM!</em></h1>
	<Results v-if="showScore" :score="score" />
	<button @click="start" :disabled="isPlaying || !isFirstTime">GO</button>
	<button @click="start" :disabled="isFirstTime || isPlaying">again</button>
	<Block v-if="isPlaying" :delay="delay" @end="endGame" />
	<Legend v-if="!isPlaying" />
</template>

<script>
import Block from './components/Block.vue';
import Results from './components/Results.vue';
import Legend from './components/Legend.vue';

export default {
	name: 'App',
	components: { Block, Results, Legend },
	data() {
		return {
			isPlaying: false,
			isFirstTime: true,
			delay: null,
			score: null,
			showScore: false,
		};
	},
	methods: {
		start() {
			this.delay = 2000 + Math.round(Math.random() * 5000); // somewhere between 2-7 seconds
			this.isPlaying = true;
			this.showScore = false;
			this.isFirstTime = false;
		},
		endGame(reactionTime) {
			// catch 2nd argument (reactionTime) of emitted custom event 'end' from Block.vue
			this.score = reactionTime;
			this.isPlaying = false;
			this.showScore = true;
		},
	},
	mounted() {
		window.addEventListener('keypress', (e) => {
			// TODO: add removeeventlistener when this.isPlaying === true
			if (e.key === ' ') {
				this.start();
			}
		});
	},
};
</script>

<style>
body {
	background: black;
	margin: 0;
}
#app {
	font-family:
		system-ui,
		-apple-system,
		BlinkMacSystemFont,
		'Segoe UI',
		Roboto,
		Oxygen,
		Ubuntu,
		Cantarell,
		'Open Sans',
		'Helvetica Neue',
		sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: yellowgreen;
	display: flex;
	align-items: center;
	height: 100vh;
	justify-content: center;
	flex-direction: column;
}
h1 {
	margin-bottom: 2em;
}
button {
	font-weight: 900;
	text-transform: uppercase;
	padding: 0.3em 1.5em;
	/* margin: 2em; */
	font-size: 2.2rem;
	border-radius: 0.3em;
	cursor: pointer;
	background-color: hsl(80, 61%, 80%);
	transition:
		background-color 0.5s ease-in-out,
		border-color 0.2s ease-in-out;
	display: inline-block;
	width: auto;
	margin-left: auto;
	margin-right: auto;
	animation: buttonblink ease-in-out 4s infinite;
	box-shadow: none;
	border-color: hsl(80, 61%, 30%);
}
button:hover {
	background-color: hsl(80, 61%, 100%);
	animation: none;
	border-color: white;
}
@keyframes buttonblink {
	0% {
		background: hsl(80, 61%, 80%);
	}
	50% {
		background: hsl(80, 61%, 100%);
	}
}
h1[disabled='true'],
button:disabled {
	display: none;
}
</style>
