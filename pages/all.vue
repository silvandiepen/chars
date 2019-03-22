<template>
	<main class="page page--home">
		<CharacterSet :data="allCharacters" />
	</main>
</template>

<script>
export default {
	components: {
		CharacterSet: () => import('~/components/character-set.vue')
	},
	computed: {
		allCharacters() {
			return this.generateCharacters();
		}
	},
	methods: {
		generateCharacters() {
			let chars = [];

			for (let i = 33; i < 1000; i++) {
				chars.push({
					code: [i],
					char: `&#${i};`,
					name: 'name',
					tags: ['tag']
				});
			}

			return chars;
		}
	}
};
</script>

<style lang="scss">
@import '~tools';

.page {
	// general page styles
	&--home {
		position: relative;
		// specific home styles
	}
}

.intro {
	height: 100vh;
	width: 100vw;
	display: flex;
	align-items: center;
	justify-content: center;
}
</style>
