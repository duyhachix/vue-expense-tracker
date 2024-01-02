<template>
	<h2 style="display: flex; justify-content: space-between">
		Expense Tracker
		<a style="cursor: pointer">
			<i
				v-if="trackerIsOpen"
				class="pi pi-chart-bar"
				style="font-size: 1.5rem"
				@click="goToGraph(false)"
			>
				Graph</i
			>
			<i v-else class="pi pi-list" style="font-size: 1.5rem" @click="goToTransactions(true)">
				Transactions</i
			>
		</a>
	</h2>
</template>

<script setup>
	import { defineProps } from 'vue';

	const props = defineProps({
		trackerIsOpen: {
			type: Boolean,
			required: true,
		},
		goToGraph: {
			type: Function,
		},
		goToTransactions: {
			type: Function,
		},
	});
</script>
