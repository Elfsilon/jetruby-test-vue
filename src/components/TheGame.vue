<template>
	<div class="game">
		<GameFinal v-if="isFinished"></GameFinal>
		<GameBoard v-else @cell-clicked="cellClicked" :chunks="splitCellsToChunks(colsCount)"></GameBoard>
		<div class="game__wrapper">
			<button @click="backToMenu" class="game__menu-button button button_size_m button_bright">Menu</button>
			<button @click="resetSettings" class="button button_size_m button_bright">Restart</button>
		</div>
	</div>
</template>

<script>
import GameBoard from '@/components/GameBoard';
import GameFinal from '@/components/GameFinal';
export default {
	props: ['rowsCount', 'colsCount'],
	components: {
		GameBoard,
		GameFinal,
	},
	data() {
		return {
			isFinished: false,
			boardClickable: true,
			previousCellData: {
				id: undefined,
				colorId: undefined,
			},
			disabledCount: 0,
			disabledGoal: (this.rowsCount * this.colsCount) / 2,
			cells: {},
			cellsArray: [],
		};
	},
	methods: {
		resetSettings: function() {
			let newCells = {};
			for (const [key, value] of Object.entries(this.cells)) {
				newCells[key] = value;
				newCells[key].disabled = false;
				newCells[key].hidden = true;
			}
			this.cells = newCells;
			this.createCellArray(this.cells);
			this.isFinished = false;
			this.disabledCount = 0;
			this.previousCellData.id = undefined;
			this.previousCellData.colorId = undefined;
		},

		backToMenu: function() {
			this.resetSettings();
			this.$emit('open-menu');
		},

		cellClicked: function(id, colorId) {
			if (this.boardClickable && !this.cells[id].disabled) {
				if (this.previousCellData.id != undefined) {
					if (id != this.previousCellData.id) {
						this.showCellColor(id);
						let prevId = this.previousCellData.id;
						if (colorId == this.previousCellData.colorId) {
							this.boardClickable = false;
							setTimeout(() => {
								this.disableCells(id, prevId);
								this.boardClickable = true;
							}, 500);
						} else {
							this.boardClickable = false;
							setTimeout(() => {
								this.hideCells(id, prevId);
								this.boardClickable = true;
							}, 500);
						}
						this.previousCellData.id = undefined;
						this.previousCellData.colorId = undefined;
					}
				} else {
					this.showCellColor(id);
					this.previousCellData.id = id;
					this.previousCellData.colorId = colorId;
				}
			}
		},

		showCellColor: function(id) {
			this.cells[id].hidden = false;
		},

		hideCells: function(id1, id2) {
			this.cells[id1].hidden = true;
			this.cells[id2].hidden = true;
		},

		disableCells(id1, id2) {
			this.cells[id1].disabled = true;
			this.cells[id2].disabled = true;
			this.cells[id1].hidden = true;
			this.cells[id2].hidden = true;
			this.disabledCount++;
			if (this.disabledCount == this.disabledGoal) this.isFinished = true;
		},

		splitCellsToChunks: function(itemsInRow) {
			let res = [];
			let tmp = [];
			for (let i = 0; i <= this.cellsArray.length; i++) {
				if (i != 0 && i % itemsInRow == 0) {
					res.push(tmp);
					tmp = [];
				}
				tmp.push(this.cellsArray[i]);
			}
			return res;
		},

		getCellData: function(id, count) {
			let colorId = id % (count / 2);
			let brightness;

			if (colorId < 8) brightness = 50;
			if (colorId >= 8 && colorId < 16) brightness = 36;
			if (colorId >= 16 && colorId < 24) brightness = 22;
			if (colorId >= 24 && colorId < 32) brightness = 8;

			return {
				id: id,
				color: `hsl(${45 * colorId}, 90%, ${brightness}%)`,
				colorId: colorId,
				hidden: true,
				disabled: false,
			};
		},

		createCells: function(count) {
			let res = {};
			for (let id = 0; id < count; id++) {
				res[id] = this.getCellData(id, count);
			}
			this.cells = Object.assign({}, res);
		},

		createCellArray: function(cells) {
			this.cellsArray = this.shuffleArray(Object.values(cells));
		},

		shuffleArray: function(array) {
			let shuffled = [...array];
			for (let i = array.length - 1; i > 0; i--) {
				const j = Math.floor(Math.random() * (i + 1));
				[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
			}
			return shuffled;
		},
	},
	mounted() {
		this.createCells(this.rowsCount * this.colsCount);
		this.createCellArray(this.cells);
	},
};
</script>

<style scoped>
.game {
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.game__wrapper {
	margin-top: 35px;
}

.game__menu-button {
	margin-right: 17px;
}
</style>
