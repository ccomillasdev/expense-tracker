<template>
	<Header />
	<div class="container">
		<Balance :total="+total" />	
		<IncomeExpenses :income="+income" :expenses="+expenses"/>
		<TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
		<AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
	</div>	
</template>

<script setup>

import { useToast } from 'vue-toastification';
import { onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
	const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
	if(savedTransactions) {
		transactions.value = savedTransactions;
	}
});

// Get total
const total = computed(() => {
	return transactions.value.reduce((acc,transaction) => {
		return acc + transaction.amount;
	},0);
});

// const income = computed(() => {
// 	return transactions.value
// 	.filter((transaction) => transaction.amount > 0)
// 	.reduce((acc,transaction) => {
// 		return acc + transaction.amount;
// 	},0).toFixed(2);
// });
const income = computed(() => {
  let totalIncome = transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
  	totalIncome = totalIncome.toFixed(2); // Return the actual number
	return totalIncome;
});

const expenses = computed(() => {
  let totalExpense = transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
	totalExpense = totalExpense * -1;
	totalExpense = totalExpense.toFixed(2); // Return the actual number
	return totalExpense;
});

// Add Transaction
const handleTransactionSubmitted = (transactionData) => {
	transactions.value.push({
		id:generateUniqueId(),
		text: transactionData.text,
		amount: transactionData.amount
	});

	savedTransactionsToLocalStorage();

	toast.success('Transaction added');
	//console.log(generateUniqueId());
}

const generateUniqueId = () => {
	return Math.floor(Math.random() * 1000000);
}

//Delete Transaction 
const handleTransactionDeleted = (id) => {
	transactions.value = transactions.value.filter((transaction) => 
	transaction.id !== id);	
	//console.log(id);

	toast.success('Transaction deleted');
};

//Save to localstorage
const savedTransactionsToLocalStorage = () => {
	localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>