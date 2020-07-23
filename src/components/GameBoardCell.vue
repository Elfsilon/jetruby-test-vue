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
	width: 50px;
	height: 50px;
	margin: 5px;
	border: 5px solid #828282;
	border-radius: 15px;
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

@media screen and (max-width: 600px) {
	.cell {
		width: 30px;
		height: 30px;
		margin: 5px;
		border: 3px solid #828282;
		border-radius: 5px;
		background-color: transparent;
		transition: all 0.35s ease-out;
	}
}
</style>
