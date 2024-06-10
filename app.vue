<template>
	<Header />
	<div class="container">
		<Balance :total="total" />	
		<IncomeExpenses :income="+income" :expenses="+expenses"/>
		<TransactionList :transactions="transactions" />
		<AddTransaction />
	</div>	
</template>

<script setup>

const transactions = ref([
	{id: 1, text: 'Flower', amount: -19.00},
	{id: 2, text: 'Salary', amount: 200.97},
	{id: 3, text: 'Book', amount: -10},
	{id: 4, text: 'Camera', amount: 150.00}
]);

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

</script>