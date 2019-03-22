<template>
	<main class="page page--home">
		<CharacterSet :data="chars" />
	</main>
</template>

<script>
export default {
	components: {
		CharacterSet: () => import('~/components/character-set.vue')
	},
	data() {
		return {
			chars: []
		};
	}
};
</script>
