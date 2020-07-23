<template>
	<div class="menu">
		<h1 class="menu__caption text text_fs_l">Memory Game</h1>
		<p class="menu__text text text_fs_s text_col_bright">Board size:</p>
		<p class="menu__text text text_fs_m">{{ rowsCount }}x{{ colsCount }}</p>
		<button @click="$emit('open-modal')" class="menu__change-size-button button button_size_s button_bright">Change</button>
		<button @click="$emit('start-game')" class="menu__start-game-button button button_size_l">Start Game</button>
	</div>
</template>

<script>
export default {
	props: ['rowsCount', 'colsCount'],
};
</script>

<style scoped>
.menu {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.menu__caption {
	margin-bottom: 52px;
}

.menu__text {
	margin-bottom: 6px;
}

.menu__change-size-button {
	margin-bottom: 52px;
}
</style>
