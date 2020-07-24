<template>
	<div class="game-wrapper">
		<transition name="fade" mode="out-in">
			<TheModal
				v-if="isModalVisible"
				@game-settings-changes="changeSettings"
				:rowsCount="settings.rowsCount"
				:colsCount="settings.colsCount"
				:visible="isModalVisible"
			></TheModal>
		</transition>
		<transition name="fade" mode="out-in">
			<TheMenu
				@start-game="startGame"
				@open-modal="openModal"
				v-if="openedPage == 'menu'"
				:rowsCount="settings.rowsCount"
				:colsCount="settings.colsCount"
			></TheMenu>
			<TheGame
				@open-menu="openMenu"
				v-if="openedPage == 'game'"
				:rowsCount="settings.rowsCount"
				:colsCount="settings.colsCount"
			></TheGame>
		</transition>
	</div>
</template>

<script>
import TheMenu from '@/components/TheMenu';
import TheGame from '@/components/TheGame';
import TheModal from '@/components/TheModal';

export default {
	name: 'App',
	components: {
		TheMenu,
		TheGame,
		TheModal,
	},
	data() {
		return {
			openedPage: 'menu',
			isModalVisible: false,
			settings: {
				rowsCount: 4,
				colsCount: 4,
			},
		};
	},
	methods: {
		openMenu: function() {
			this.openedPage = 'menu';
		},

		openModal: function() {
			this.isModalVisible = true;
		},

		startGame: function() {
			this.openedPage = 'game';
		},

		changeSettings: function(newRowCount, newColCount) {
			this.settings.rowsCount = newRowCount;
			this.settings.colsCount = newColCount;
			this.isModalVisible = false;
		},
	},
};
</script>

<style>
body {
	font-family: 'Rubik', sans-serif;
}

* {
	box-sizing: border-box;
	margin: 0;
	padding: 0;
}

/* Fade transition */
.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.22s;
}

.fade-enter,
.fade-leave-to {
	opacity: 0;
}

/* Local */
.game-wrapper {
	width: 100%;
	height: 100vh;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

/* Global styles */
/* Text */
.text {
	color: #5e5e5e;
}

.text_fs_xs {
	font-size: 20px;
}

.text_fs_s {
	font-size: 25px;
}

.text_fs_m {
	font-size: 40px;
}

.text_fs_l {
	font-size: 50px;
}

.text_col_bright {
	color: #828282;
}

@media screen and (max-width: 600px) {
	.text_fs_xs {
		font-size: 14px;
	}

	.text_fs_s {
		font-size: 18px;
	}

	.text_fs_m {
		font-size: 25px;
	}

	.text_fs_l {
		font-size: 30px;
	}
}

/* Button */
.button {
	color: #5e5e5e;
	border-color: #5e5e5e;
	border-style: solid;
	background: none;
	cursor: pointer;
	outline: none;
	transition: all 0.35s ease;
}

.button:hover {
	border-color: #10cde7;
	color: #10cde7;
}

.button_size_s {
	padding: 11px 23px;
	border-width: 3px;
	border-radius: 20px;
	font-size: 25px;
}

.button_size_m {
	padding: 12px 43px;
	border-width: 5px;
	border-radius: 25px;
	font-size: 30px;
}

.button_size_l {
	padding: 14px 63px;
	border-width: 5px;
	border-radius: 30px;
	font-size: 35px;
}

.button_bright {
	color: #b8b8b8;
	border-color: #b8b8b8;
}

@media screen and (max-width: 600px) {
	.button_size_s {
		padding: 6px 15px;
		border-width: 2px;
		border-radius: 20px;
		font-size: 20px;
	}

	.button_size_m {
		padding: 8px 25px;
		border-width: 3px;
		border-radius: 25px;
		font-size: 25px;
	}

	.button_size_l {
		padding: 10px 35px;
		border-width: 3px;
		border-radius: 30px;
		font-size: 30px;
	}
}
</style>
