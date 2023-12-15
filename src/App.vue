<template>
  <div>My app</div>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
//Anything you want it to be reactive you wrap it in ref function:
import { ref, computed } from "vue";
const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.33 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);
// Get total
const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => acc + transaction.amount,
    0
  );
});
// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0)
    .toFixed(2);
});
// Get expenses
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0)
    .toFixed(2);
});
</script>
