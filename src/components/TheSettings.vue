<template>
	<div class="settings" :class="{ settings_highlight_wrong: isSelectWrong }">
		<div class="settings__board" @mouseleave="setMouseCoords(rowSelected, colSelected)">
			<div class="settings__row" v-for="i in maxBoardRows" :key="i" @mouseenter="setMouseOverRow(i)">
				<div
					class="settings__cell"
					:class="{
						settings__cell_highlight: cellInSelectedArea(i, j),
						settings__cell_highlight_wrong: cellInWrongSelectedArea(i, j),
					}"
					v-for="j in maxBoardCols"
					:key="j"
					@mouseenter="setMouseOverCol(j)"
					@click="selectSize"
				></div>
			</div>
		</div>
		<div class="settings__info">
			<h3 class="settings__caption text text_fs_xs">Board size:</h3>
			<p class="settings__size text text_fs_xs">{{ rowSelected }}x{{ colSelected }}</p>
			<button @click="emitSettingsChanges" class="button button_size_s">Apply</button>
		</div>
	</div>
</template>

<script>
export default {
	props: ['rowsCount', 'colsCount'],
	data() {
		return {
			maxBoardRows: window.innerWidth > 600 ? 8 : 6,
			maxBoardCols: window.innerWidth > 600 ? 8 : 6,
			mouseOverRow: 4,
			mouseOverCol: 4,
			rowSelected: this.rowsCount,
			colSelected: this.colsCount,
			isSizeRight: true,
			isSelectWrong: false,
		};
	},
	methods: {
		selectSize: function() {
			if ((this.mouseOverCol * this.mouseOverRow) % 2 == 0) {
				this.rowSelected = this.mouseOverRow;
				this.colSelected = this.mouseOverCol;
			} else {
				this.isSelectWrong = true;
				setTimeout(() => {
					this.isSelectWrong = false;
				}, 350);
			}
		},

		setMouseCoords(row, col) {
			this.setMouseOverRow(row);
			this.setMouseOverCol(col);
		},

		setMouseOverRow: function(val) {
			this.mouseOverRow = val;
		},

		setMouseOverCol: function(val) {
			this.mouseOverCol = val;
		},

		cellInSelectedArea: function(i, j) {
			return j <= this.mouseOverCol && i <= this.mouseOverRow;
		},

		cellInWrongSelectedArea: function(i, j) {
			return this.cellInSelectedArea(i, j) && (this.mouseOverCol * this.mouseOverRow) % 2 != 0;
		},

		emitSettingsChanges: function() {
			this.$emit('game-settings-changes', this.rowSelected, this.colSelected);
		},
	},
};
</script>

<style scoped>
.settings {
	background-color: white;
	border-radius: 20px;
	padding: 30px 40px;
	display: flex;
	align-items: center;
}

.settings__board {
	margin-right: 30px;
	background-color: transparent;
	border-radius: 15px;
	transition: all 0.35s ease;
}

.settings__row {
	width: 100%;
	display: flex;
}

.settings__cell {
	width: 30px;
	height: 30px;
	border: 2px solid #828282;
	border-radius: 7px;
	margin: 2px;
	transition: border-color 0.25s ease;
}

.settings__info {
	height: 100%;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.settings__caption {
	margin-bottom: 4px;
}

.settings__size {
	margin-bottom: 28px;
}

.settings__cell_highlight {
	border-color: #10cde7;
}

.settings__cell_highlight_wrong {
	border-color: #e71034;
}

.settings_highlight_wrong {
	background-color: rgb(238, 148, 163);
}

@media screen and (max-width: 600px) {
	.settings {
		background-color: white;
		border-radius: 20px;
		padding: 20px 30px;
		display: flex;
		height: 244px;
	}

	.settings__board {
		width: 204px;
		margin-right: 30px;
	}

	.settings__caption {
		margin-bottom: 3px;
	}

	.settings__size {
		margin-bottom: 18px;
	}
}

@media screen and (max-width: 380px) {
	.settings {
		background-color: white;
		border-radius: 20px;
		padding: 10px 15px;
		display: flex;
		height: 224px;
	}

	.settings__board {
		width: 204px;
		margin-right: 10px;
	}

	.settings__caption {
		margin-bottom: 3px;
	}

	.settings__size {
		margin-bottom: 18px;
	}
}
</style>
