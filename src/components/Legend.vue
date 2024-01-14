<script>
export default {
	data() {
		return {
			text: {
				en: {
					keyboard: 'Keyboard',
					space: 'space',
					toreplay: 'to (re-play)',
					enter: 'enter',
					toreact: 'to react',
				},
				pt: {
					keyboard: 'Teclado',
					space: 'espaço',
					toreplay: 'que (jogar de novo)',
					enter: 'entrar',
					toreact: 'tempo de reação reagir',
				},
			},
			language: localStorage.getItem('language'),
		};
	},
};
</script>
<template>
	<div>
		{{
			language === 'pt' ? text.pt.keyboard : text.en.keyboard
		}}:<br />&lt;<span class="lighter">{{
			language === 'pt' ? text.pt.space : text.en.space
		}}</span
		>&gt; {{ language === 'pt' ? text.pt.toreplay : text.en.toreplay }},
		&lt;<span class="lighter">{{
			language === 'pt' ? text.pt.enter : text.en.enter
		}}</span
		>&gt; {{ language === 'pt' ? text.pt.toreact : text.en.toreact }}
	</div>
</template>

<style scoped>
div {
	margin-top: 3rem;
	color: hsl(180, 50%, 15%);
}
.lighter {
	color: hsl(180, 50%, 25%);
}
</style>
