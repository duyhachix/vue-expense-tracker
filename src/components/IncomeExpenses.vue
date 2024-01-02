<template>
	<div class="inc-exp-container">
		<div>
			<h4>Income</h4>
			<p id="money-plus" class="money plus">+${{ income }}</p>
		</div>
		<div>
			<h4>Expense</h4>
			<p id="money-minus" class="money minus">-${{ positiveExpenseNumber }}</p>
		</div>
	</div>
</template>

<script setup>
	import { computed } from 'vue';
	import { defineProps } from 'vue';

	const props = defineProps({
		income: {
			type: Number,
			required: true,
		},
		expenses: {
			type: Number,
			required: true,
		},
	});
  
	let positiveExpenseNumber = computed(() => {
		return Math.abs(props.expenses);
	});
</script>
