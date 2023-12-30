<template>
	<p class="reaction-time">Reaction time: {{ score }}ms</p>
	<p class="bigger">{{ rank }}</p>
</template>

<script>
export default {
	props: ['score'],
	data() {
		return {
			rank: null,
		};
	},
	mounted() {
		if (this.score < 200) {
			this.rank = 'Nao e normal em!';
		} else if (this.score < 250) {
			this.rank = 'rapido!';
		} else if (this.score < 300) this.rank = 'oke oke';
		else if (this.score < 350) {
			this.rank = 'bit average';
		} else if (this.score < 500) this.rank = 'meh';
		else this.rank = 'slome, try again';
	},
};
</script>

<style>
.reaction-time {
	text-shadow: 2px 2px 0px rgba(255, 255, 255, 0.2);
	margin-bottom: 0;
	color: white;
}
.bigger {
	margin-top: 0;
	font-size: 2.5em;
	font-weight: bold;
	text-shadow: 2px 2px 0px red;
	color: white;
	font-style: italic;
}
</style>
