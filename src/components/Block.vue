<template>
	<div class="backdrop">
		<div id="be-ready" v-if="showBlock === false">
			{{ language === 'pt' ? text.pt.beready : text.en.beready }}
			<div class="subtext">
				{{
					language === 'pt' ? text.pt.bereadysub : text.en.bereadysub
				}}
			</div>
		</div>
		<div id="block" v-if="showBlock" @click="stopTimer">
			<div>
				{{ language === 'pt' ? text.pt.click : text.en.click }}
				<i class="small"
					><br />{{
						language === 'pt'
							? text.pt.shownafter
							: text.en.shownafter
					}}
					{{ delay }}ms</i
				>
			</div>
		</div>
	</div>
</template>
<script>
import { ref } from 'vue';
export default {
	props: ['delay'],
	data() {
		return {
			showBlock: ref(false),
			timer: null,
			reactionTime: 0,
			text: {
				en: {
					beready: 'Be ready',
					bereadysub: 'Click anywhere or press ENTER soon...',
					click: 'Click!',
					shownafter: 'Shown after ',
				},
				pt: {
					beready: 'Preparado?',
					bereadysub: 'clique ou aperte ENTER em breve...',
					click: 'clique!',
					shownafter: 'mostrado após',
				},
			},
			language: localStorage.getItem('language'),
		};
	},
	created() {
		setTimeout(() => {
			this.showBlock = true;
			this.startTimer();
		}, this.delay);
	},
	methods: {
		startTimer() {
			this.timer = setInterval(() => {
				this.reactionTime += 10;
			}, 10);
		},
		stopTimer() {
			clearInterval(this.timer);
			this.showBlock = false;
			this.$emit('end', this.reactionTime);
			window.removeEventListener('keypress', (e) => this.keyboard(e));
		},
		keyboard(e = []) {
			if (e.key === 'Enter') {
				this.stopTimer();
			}
		},
	},
	updated() {
		if (this.showBlock === true) {
			window.addEventListener('keypress', (e) => this.keyboard(e));
		}
	},
};
</script>

<style>
.backdrop {
	top: 0;
	position: fixed;
	height: 100vh;
	width: 100vw;
	background: rgba(100, 100, 100, 0.3);
	display: flex;
	align-items: center;
	flex-wrap: wrap;
	box-sizing: border-box;
	padding: 1rem;
}

#be-ready {
	font-style: italic;
	color: rgba(255, 255, 255, 0.5);
	text-align: center;
	font-size: 3rem;
	text-transform: lowercase;
	margin: 0 auto;
	animation-name: get-ready;
	animation-duration: 2s;
	animation-iteration-count: infinite;
	animation-timing-function: ease-in-out;
}
#be-ready .subtext {
	font-size: 0.35em;
}
@keyframes get-ready {
	0% {
		opacity: 100%;
	}
	50% {
		opacity: 20%;
	}
}

#block {
	width: 100vw;
	height: 100vh;
	background: hsl(0, 100%, 50%);
	color: white;
	display: flex;
	flex-wrap: wrap;
	align-items: center;
	justify-content: center;
	font-size: 8rem;
	text-transform: uppercase;
}
@media screen and (max-width: 600px) {
	#block {
		font-size: 4rem;
	}
}

.small {
	font-size: 1rem;
	width: 100%;
	color: hsl(0, 100%, 75%);
	display: block;
}
</style>
