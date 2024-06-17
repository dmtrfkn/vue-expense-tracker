<template>
  <Header />
  <div class="container">
    <Balance :totalBalance="total" />
    <IncomeExpenses :income="+positive" :expense="+negative" />
    <TransactionList @transactionDeleted="handleTransactionDeleted" :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from '@/components/Header.vue';
import Balance from '@/components/Balance.vue';
import IncomeExpenses from '@/components/IncomeExpenses.vue';
import TransactionList from '@/components/TransactionList.vue';
import AddTransaction from '@/components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const positive = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
const negative = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: Date.now(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionsToLocalStorage();

  toast.success('Transaction Added!');
};

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  saveTransactionsToLocalStorage();

  toast.success('Transaction Deleted!');
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) transactions.value = savedTransactions;
});
</script>

<style lang="scss" scoped></style>
