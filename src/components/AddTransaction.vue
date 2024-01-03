<template>
	<h3>Add new transaction</h3>
	<form id="form" @submit.prevent="onSubmit">
		<div class="form-control">
			<label for="text">Text</label>
			<input type="text" id="text" placeholder="Enter text..." v-model="text" />
		</div>
		<div class="form-control">
			<label for="amount"
				>Amount <br />
				(negative - expense, positive - income)</label
			>
			<input type="text" id="amount" placeholder="Enter amount..." v-model="amount" />
		</div>
		<button :disabled="!isFullFilled" :class="{ 'btn-disabled': !isFullFilled }" class="btn">
			Add transaction
		</button>
	</form>
</template>

<script setup>
	import { useToast } from 'vue-toastification';
	import { ref, computed } from 'vue';

	const text = ref('');
	const amount = ref('');

	// Get toast interface
	const toast = useToast();

	const emit = defineEmits(['transactionSubmitted']);

	let isFullFilled = computed(() => {
		return text.value && amount.value;
	});

	const onSubmit = () => {
		if (!text.value || !amount.value) {
			// Display a toast error message if either field is empty
			toast.error('Both fields must be filled.');
			return;
		}

		const transactionData = {
			text: text.value,
			amount: parseFloat(amount.value),
		};

		if (!transactionData['amount']) {
			toast.info('Amount must be a number.', {
				position: 'top-right',
				timeout: 5000,
				closeOnClick: true,
				pauseOnFocusLoss: true,
				draggable: true,
				draggablePercent: 0.6,
				closeButton: 'button',
			});
			return;
		}

		emit('transactionSubmitted', transactionData);

		// Clear form fields
		text.value = '';
		amount.value = '';
	};
</script>
