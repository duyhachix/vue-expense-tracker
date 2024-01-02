<template>
	<Pie :data="data" :options="options" />
</template>

<script setup>
	import { ref, onMounted, defineProps } from 'vue';
	import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';
	import { Pie } from 'vue-chartjs';

	ChartJS.register(ArcElement, Tooltip, Legend);

	let props = defineProps({
		income: {
			type: Number,
			required: true,
		},
		expenses: {
			type: Number,
			required: true,
		},
	});

	const data = ref({
		labels: ['Income', 'Expense'],
		datasets: [
			{
				backgroundColor: ['#2ecc71', '#c0392b'],
				data: [props.income, Math.abs(props.expenses)],
			},
		],
	});
	const options = ref({
		responsive: true,
		legend: {
			position: 'right',
		},
		// maintainAspectRatio: false,
	});
</script>
