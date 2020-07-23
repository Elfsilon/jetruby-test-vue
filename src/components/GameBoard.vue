<template>
	<div class="board">
		<GameBoardRow @cell-clicked="emitCellClick" v-for="chunk in chunks" :key="chunk[0].id" :cells="chunk"></GameBoardRow>
	</div>
</template>

<script>
import GameBoardRow from '@/components/GameBoardRow';
export default {
	props: ['chunks'],
	components: {
		GameBoardRow,
	},
	methods: {
		emitCellClick: function(id, colorId) {
			this.$emit('cell-clicked', id, colorId);
		},
	},
};
</script>
