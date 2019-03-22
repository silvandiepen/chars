<template>
	<main class="page page--home">
		<CharacterSet :data="emojiData" />
	</main>
</template>

<script>
const emojiData = require('~/data/emoji.json');

export default {
	components: {
		CharacterSet: () => import('~/components/character-set.vue')
	},
	data() {
		return {
			emojiData: emojiData
		};
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
