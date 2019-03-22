<template>
	<section class="char__overview">
		<div class="row"></div>
		<div class="column char__column">
			<div class="char__search">
				<input v-model="searchTerm" type="search" placeholder="search.."  >
			</div>
			<ul class="char__tags">
				<span class="char__totaltags"> {{ tags.length }}</span>
				<li v-for="(tag, index) in tags" :key="index" class="char__tag">
					<span class="char__tag-text" @click="searchTerm = tag">{{ tag }}</span>
				</li>
			</ul>
			<div v-if="selectedCharacter" class="char__detail">
				<h4>{{ selectedCharacter.name }}</h4>
				<div class="char__detail-character">{{ selectedCharacter.char }}</div>
				<ul class="char__detail-tags">
					<li v-for="(tag, index) in selectedCharacter.tags" :key="index" class="char__tag">
						<span class="char__tag-text" @click="searchTerm = tag">{{ tag }}</span>
					</li>
				</ul>
			</div>

			<transition-group v-show="history.length > 0" class="char__list char__history-list" name="list2" tag="ul">
				<li
					v-for="(char, index) in history"
					:key="index"
					class="char__item char__history-item"
					@click="selectCharacter(char)"
				>
					<span class="char__character">
						{{ char.char }}
					</span>
				</li>
			</transition-group>

			<transition-group v-show="characters.length > 0" name="list" tag="ul" class="char__list">
				<li v-for="(char, index) in characters" :key="index" class="char__item" @click="selectCharacter(char)">
					<span :data-char="char.code[0]" class="char__character">
						<!-- {{ toChar(char.code[0]) }} -->
						{{ char.char }}
						<!-- {{ char.code[0] }} -->
					</span>
				</li>
			</transition-group>
			<div v-if="characters.length < 1" class="char__empty">
				Nothing found...
			</div>
		</div>
	</section>
</template>

<script>
export default {
	props: {
		data: {
			type: Array,
			default: () => []
		}
	},
	data() {
		return {
			selectedCharacter: null,
			history: [],
			searchTerm: null
		};
	},
	computed: {
		characters() {
			const _this = this;
			let characters = this.$props.data;
			if (_this.searchTerm !== null || _this.searchTerm !== '' || !_this.searchTerm) {
				characters = this.$props.data.filter(function(item) {
					return item.tags.includes(_this.searchTerm);
				});
			}
			return characters;
		},
		tags() {
			const _this = this;
			// Reduce all to just the tags
			let tags = _this.$props.data.reduce(function(a, b) {
				return a.concat(b.tags);
			}, []);

			// Remove double tags
			tags = tags.filter(function(item, pos) {
				return tags.indexOf(item) == pos;
			});

			// Remove all tags with spaces;
			tags = tags.filter(function(item) {
				return item.indexOf(' ') < 1 && item.indexOf('”') < 1 && item.indexOf('(') < 1;
			});

			// Order tags by alphabet
			tags = tags.sort();

			// Filer tags by search term

			if (_this.searchTerm !== null || _this.searchTerm !== '' || !_this.searchTerm) {
				tags = tags.filter(function(item) {
					return item.indexOf(_this.searchTerm) > -1;
				});
			} else {
				tags = [];
			}

			return tags;
		}
	},
	methods: {
		toChar(char) {
			let hexes = char.match(/.{1,4}/g) || [];
			let value = '';
			for (let i = 0; i < hexes.length; i++) {
				value += String.fromCharCode(parseInt(hexes[i], 16));
			}
			return value;
		},
		selectCharacter(char) {
			const _this = this;
			_this.selectedCharacter = char;
			if (_this.history.includes(char)) {
				_this.history.splice(_this.history.indexOf(char) - 1, 1);
				setTimeout(() => {
					_this.history.push(char);
				}, 100);
			} else {
				_this.history.push(char);
			}
		}
	}
};
</script>

<style lang="scss">
@import '~tools';

.char {
	&__column {
		width: 100%;
	}
	&__overview {
		background-color: color(Offwhite);
	}
	&__list {
		margin: 0;
		width: 100%;
		display: grid;
		padding: 20px;
		grid-gap: 20px;
		grid-template-columns: repeat(8, 1fr);
		@media #{$large-down} {
			grid-template-columns: repeat(6, 1fr);
		}
		@media #{$small-only} {
			grid-template-columns: repeat(4, 1fr);
		}
		max-height: calc(100vh - 320px);
		overflow: scroll;
	}
	&__item {
		width: calc((100vw - (9 * 20px)) / 8);
		@media #{$large-down} {
			width: calc((100vw - (7 * 20px)) / 6);
		}
		@media #{$small-only} {
			width: calc((100vw - (5 * 20px)) / 4);
		}
		height: 0;
		padding-bottom: 100%;
		background-color: color(White);
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		box-shadow: 0 0 20px 0 color(Black, 0);
		cursor: pointer;
		transition: box-shadow 0.3s;

		&:hover {
			box-shadow: 0 0 20px 0 color(Black, 0.1);
		}
	}
	&__detail {
		background-color: color(White);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 20px;
		color: color(Black);
		min-height: grid(4);
		@media #{$small-only} {
			height: calc((100vw / 16) * 9);
		}
	}
	&__detail-character {
		display: block;
		font-size: 120px;
		padding: 2rem;
	}
	&__character {
		display: block;
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		font-size: 2rem;

		&:before {
			display: block;
			// border: 1px solid red;
			// content: attr(data-char);
		}
	}
	&__tags {
		width: 100%;
		left: 0;
		top: 0;
		margin: 0;
		z-index: 10;
		padding: 2rem;
		background-color: color(Black);
		color: color(White);
	}
	&__tag {
		display: inline-block;
		padding: 0.25rem;
		&:before {
			content: '#';
			color: color(Purple);
		}
		cursor: pointer;
	}
	&__totaltags {
		display: inline-block;
		background-color: color(Purple);
		border-radius: 2px;
		padding: 0.25rem 0.5rem;
		font-size: 12px;
	}
	&__empty {
		padding: 2rem;
		width: 100%;
		text-align: center;
	}
	&__search {
		background-color: color(Purple);
		width: 100%;
		input {
			width: 100%;
			height: 100%;
			padding: 2rem;
			border: none;
			background-color: transparent;
			font-size: 2rem;
			color: white;
			&::placeholder {
				color: white;
				opacity: 0.5;
			}
			&:focus {
				outline: none;
			}
		}
	}

	&__history-list {
		grid-template-columns: repeat(100, 1fr);
		overflow: auto;
		border-bottom: 1px solid color(Black, 0.1);
	}
	&__history-item {
		cursor: pointer;
		width: 80px;
		// background-color: color(Black);
	}
}

.list2-item,
.list-item {
	display: inline-block;
	margin-right: 10px;
}
.list2-enter-active,
.list2-leave-active,
.list-enter-active,
.list-leave-active {
	transition: all 0.3s;
}
.list2-enter, .list2-leave-to,
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
	opacity: 0;
	transform: scale(0);
}
</style>
