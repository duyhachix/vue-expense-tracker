<template>
	<Header
		:trackerIsOpen="trackerIsOpen"
		:goToGraph="goToGraph"
		:goToTransactions="goToTransactions"
	/>
	<div v-if="trackerIsOpen">
		<div class="container expense-tracker">
			<Balance :total="total" />
			<IncomeExpenses :income="+income" :expenses="+expenses" />
			<TransactionList
				:transactions="transactions"
				@transactionDeleted="handleTransactionDeleted"
			/>
			<AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
		</div>
	</div>
	<div v-else class="container expense-graph">
		<IncomeExpenses :income="+income" :expenses="+expenses" />
		<Graph :income="+income" :expenses="+expenses"></Graph>
	</div>
</template>

<script setup>
	import Header from './components/Header.vue';
	import Balance from './components/Balance.vue';
	import IncomeExpenses from './components/IncomeExpenses.vue';
	import TransactionList from './components/TransactionList.vue';
	import AddTransaction from './components/AddTransaction.vue';
	import Graph from './components/Graph.vue';

	import { ref, computed, onMounted } from 'vue';

	import { useToast } from 'vue-toastification';

	const toast = useToast();

	const transactions = ref([]);

	let trackerIsOpen = ref(true);

	function goToGraph(value) {
		trackerIsOpen.value = value;
	}

	function goToTransactions(value) {
		trackerIsOpen.value = value;
	}

	onMounted(() => {
		const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

		if (savedTransactions) {
			transactions.value = savedTransactions;
		}
	});

	// Get total
	const total = computed(() => {
		return transactions.value.reduce((acc, transaction) => {
			return acc + transaction.amount;
		}, 0);
	});

	// Get income
	const income = computed(() => {
		return transactions.value
			.filter((transaction) => transaction.amount > 0)
			.reduce((acc, transaction) => acc + transaction.amount, 0)
			.toFixed(2);
	});

	// Get expenses
	const expenses = computed(() => {
		return transactions.value
			.filter((transaction) => transaction.amount < 0)
			.reduce((acc, transaction) => acc + transaction.amount, 0)
			.toFixed(2);
	});

	// Submit transaction
	const handleTransactionSubmitted = (transactionData) => {
		transactions.value.unshift({
			id: generateUniqueId(),
			text: transactionData.text,
			amount: transactionData.amount,
		});

		saveTransactionsToLocalStorage();

		toast.success('Transaction added.');
	};

	// Generate unique ID
	const generateUniqueId = () => {
		return Math.floor(Math.random() * 1000000);
	};

	// Delete transaction
	const handleTransactionDeleted = (id) => {
		transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

		saveTransactionsToLocalStorage();

		toast.success('Transaction deleted.');
	};

	// Save transactions to local storage
	const saveTransactionsToLocalStorage = () => {
		localStorage.setItem('transactions', JSON.stringify(transactions.value));
	};
</script>
