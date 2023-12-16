<template>
  <div>My app</div>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="deleteTransaction"
    />
    <AddTransaction @transactionSubmitted="transactionSubmitted" />
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";
//Anything you want it to be reactive you wrap it in ref function:
import { ref, computed, onMounted } from "vue";
const transactions = ref([]);
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
const toast = useToast();
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
const transactionSubmitted = (data) => {
  //We need to use a helper function to generate a unique id for us
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  });
  //  A toast to verify the input
  saveTransactionsToLocalStorage();
  toast.success("Transaction added");
};
// delete transaction
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
  toast.success("Transaction deleted");
};
// Save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000000);
};
</script>
