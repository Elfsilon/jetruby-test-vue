<template>
	<div
		class="cell"
		@click="$emit('cell-clicked', state.id, state.colorId)"
		:class="{ cell_disabled: state.disabled }"
		:style="{ backgroundColor: currentColor }"
	></div>
</template>

<script>
export default {
	props: ['state'],
	computed: {
		currentColor: function() {
			return this.state.hidden ? 'transparent' : this.state.color;
		},
	},
};
</script>

<style scoped>
.cell {
	width: 60px;
	height: 60px;
	margin: 5px;
	border: 5px solid #828282;
	border-radius: 20px;
	background-color: transparent;
	transition: all 0.35s ease-out;
}

.cell_border_s {
	border-width: 3px;
	border-radius: 10px;
}

.cell_disabled {
	opacity: 0;
}
</style>
