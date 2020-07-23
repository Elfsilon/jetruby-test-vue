<template>
	<div class="modal">
		<TheSettings
			@game-settings-changes="emitSettingsChanges"
			v-if="content == 'settings'"
			:rowsCount="rowsCount"
			:colsCount="colsCount"
		></TheSettings>
	</div>
</template>

<script>
import TheSettings from '@/components/TheSettings';
export default {
	props: ['rowsCount', 'colsCount'],
	components: {
		TheSettings,
	},
	data() {
		return {
			content: 'settings',
		};
	},
	methods: {
		emitSettingsChanges: function(newRowCount, newColCount) {
			this.$emit('game-settings-changes', newRowCount, newColCount);
		},
	},
};
</script>

<style scoped>
.modal {
	position: fixed;
	left: 0;
	top: 0;
	width: 100%;
	height: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	background-color: rgba(65, 64, 64, 0.6);
}

.modal_hidden {
	display: none;
}
</style>
