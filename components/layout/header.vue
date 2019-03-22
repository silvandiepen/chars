<template>
	<header id="header" class="header">
		<div class="row">
			<div class="column small-full medium-half">
				<h3>Chars</h3>
			</div>
			<div class="column small-full medium-half">
				<nuxt-link to="/all">All characters</nuxt-link>
				<nuxt-link to="/emoji">Emoji</nuxt-link>
			</div>
		</div>
	</header>
</template>

<script>
export default {};
</script>

<style lang="scss">
@import '~tools';

.header {
	padding: 2rem;
}
</style>
