<script>
import Block from './components/Block.vue'
import Results from './components/Results.vue'
import Legend from './components/Legend.vue'

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
			text: {
				en: {
					title: 'Think fast',
					subtitle:
						'Click anywhere or press ENTER as fast as possible.',
					action: 'Go',
					again: 'again',
				},
				pt: {
					title: 'Você é rápido?',
					action: 'sim, vai',
					subtitle:
						'Clique em qualquer lugar ou aperte Enter o mais rápido possível.',
					again: 'de novo',
				},
			},
			language: localStorage.getItem('language'),
		}
	},
	methods: {
		start() {
			this.delay = 2000 + Math.round(Math.random() * 5000) // somewhere between 2-7 seconds
			this.isPlaying = true
			this.showScore = false
			this.isFirstTime = false
		},
		endGame(reactionTime) {
			// catch 2nd argument (reactionTime) of emitted custom event 'end' from Block.vue
			this.score = reactionTime
			this.isPlaying = false
			this.showScore = true
		},
	},
	beforeCreate() {
		if (!localStorage.getItem('language')) {
			if (navigator.language.substring(0, 2) === 'pt') {
				localStorage.setItem('language', 'pt')
				document.title = 'Pensar rápido!'
			} else localStorage.setItem('language', 'en')
		}
	},
	mounted() {
		window.addEventListener('keypress', (e) => {
			if (this.isPlaying === false && e.key === ' ') this.start()
		})
	},
}
</script>
<template>
	<!-- TODO use dynamic path for logo, for use within ckdev88.github.io -->
	<img v-if="!isPlaying" src="/thinkfast/thinkfast-light.svg" id="logo" alt=""/>
	<h1 :disabled="isPlaying || !isFirstTime">
		{{ language === 'pt' ? text.pt.title : text.en.title }}
		<em>{{ language === 'pt' ? text.pt.subtitle : text.en.subtitle }}</em>
	</h1>
	<Results v-if="showScore" :score="score" />
	<button @click="start" :disabled="isPlaying || !isFirstTime">
		{{ language === 'pt' ? text.pt.action : text.en.action }}
	</button>
	<button @click="start" :disabled="isFirstTime || isPlaying">
		{{ language === 'pt' ? text.pt.again : text.en.again }}
	</button>
	<Block v-if="isPlaying" :delay="delay" @end="endGame" />
	<Legend v-if="!isPlaying" />
</template>

<style>
body {
	background: black;
	margin: 0;
}
#app {
	font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI',
		Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue',
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
	padding: 1rem;
	box-sizing: border-box;
}
h1 {
	margin-bottom: 2em;
}
button {
	font-weight: bold;
	text-transform: uppercase;
	padding: 0.5em 1em;
	font-size: 2.2rem;
	border-radius: 3.5em;
	cursor: pointer;
	background-color: hsl(80, 61%, 80%);
	transition: background-color 0.5s ease-in-out, border-color 0.2s ease-in-out;
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
h1 em {
	display: block;
	font-style: italic;
	font-size: 0.5em;
	opacity: 0.6;
}
#logo {
	width: 100px;
	height: auto;
	opacity: 0.3;
	animation: logo-animation 4s infinite;
}
@keyframes logo-animation {
	0% {
		opacity: 0.3;
	}
	50% {
		opacity: 0.7;
	}
}
</style>
