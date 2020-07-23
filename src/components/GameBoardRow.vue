<template>
	<div class="row">
		<GameBoardCell @cell-clicked="emitCellClick" v-for="cell of cells" :key="cell.id" :state="cell"></GameBoardCell>
	</div>
</template>

<script>
import GameBoardCell from '@/components/GameBoardCell';
export default {
	props: ['cells'],
	components: {
		GameBoardCell,
	},
	methods: {
		emitCellClick: function(id, colorId) {
			this.$emit('cell-clicked', id, colorId);
		},
	},
};
</script>

<style scoped>
.row {
	display: flex;
	justify-content: center;
	margin-bottom: 5px;
}
</style>
