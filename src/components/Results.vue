<template>
	<p class="reaction-time">
		{{ language === 'pt' ? text.pt.reactiontime : text.en.reactiontime }}:
		{{ score }}ms
	</p>
	<p class="bigger">{{ rank }}</p>
	<p>
		{{ language === 'pt' ? text.pt.topscore : text.en.topscore }}:
		{{ topscore }}ms
	</p>
</template>

<script>
export default {
	props: ['score', 'topscore'],
	data() {
		return {
			rank: null,
			text: {
				en: {
					reactiontime: 'Reaction time',
					rank200: 'wow, not normal!',
					rank250: 'pretty quick',
					rank300: 'okay not bad',
					rank350: 'bit average',
					rank500: 'meh',
					rank600: "that's bad, try again",
					topscore: 'Top score so far',
				},
				pt: {
					reactiontime: 'Sua velocidade',
					rank200: 'não é normal hein!',
					rank250: 'rápido!',
					rank300: 'taa bom',
					rank350: 'foi mais ou menos, tenta de novo',
					rank500: 'dá para melhorar',
					rank600: 'muito devagar, tenta de novo',
					topscore: 'Melhores score',
				},
			},
			language: localStorage.getItem('language'),
		}
	},
	mounted() {
		if (this.score < 200) {
			if (this.language === 'pt') this.rank = this.text.pt.rank200
			else this.rank = this.text.en.rank200
		} else if (this.score < 250) {
			if (this.language === 'pt') this.rank = this.text.pt.rank250
			else this.rank = this.text.en.rank250
		} else if (this.score < 300)
			if (this.language === 'pt') this.rank = this.text.pt.rank300
			else this.rank = this.text.en.rank300
		else if (this.score < 350) {
			if (this.language === 'pt') this.rank = this.text.pt.rank350
			else this.rank = this.text.en.rank350
		} else if (this.score < 500) {
			if (this.language === 'pt') this.rank = this.text.pt.rank500
			else this.rank = this.text.en.rank500
		} else {
			if (this.language === 'pt') this.rank = this.text.pt.rank600
			else this.rank = this.text.en.rank600
		}
	},
}
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
