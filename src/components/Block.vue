<template>
	<div class="backdrop">
		<div id="be-ready" v-if="showBlock === false">Be ready</div>
		<div class="block" v-if="showBlock" @click="stopTimer">
			<div class="inside">
				Click!
				<i class="small"><br />Shown after {{ delay }}ms</i>
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
		if (this.showBlock === false) {
			console.log('this.showBlock changed to:', this.showBlock);
			// TODO: remove event listener voor Enter
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
	/* opacity: 30%; */
}
@keyframes get-ready {
	0% {
		opacity: 100%;
	}
	50% {
		opacity: 20%;
	}
	100% {
		opacity: 100%;
	}
}

.block {
	width: 100vw;
	height: 100vh;
	background: red;
	color: white;
	display: flex;
	flex-wrap: wrap;
	align-self: center;
	justify-content: center;
	font-size: 8rem;
	text-transform: uppercase;
}
@media screen and (max-width: 600px) {
	.block {
		font-size: 4rem;
	}
}

.inside {
	align-self: center;
}

.small {
	font-size: 1rem;
	width: 100%;
	color: lightcoral;
	display: block;
}
</style>
