<script setup>
import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";
import { ref, computed, onMounted } from "vue";

const transactions = ref([
  { id: 1, text: "testing", amount: 20 },
  { id: 2, text: "Lambo", amount: -30 },
]);

onMounted(() => {
  const data = JSON.parse(localStorage.getItem("transactions"));
  if (data) {
    transactions.value = data;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((e) => e.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});
const expenses = computed(() => {
  return transactions.value
    .filter((e) => e.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

const text = ref("");

function DeleteTransaction(transactionId) {
  transactions.value = transactions.value.filter((e) => e.id !== transactionId);
  SaveTolocalStorage();
}
function SubmitTransaction(transactionData) {
  transactions.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  SaveTolocalStorage();
}

function SaveTolocalStorage() {
  return localStorage.setItem(
    "transactions",
    JSON.stringify(transactions.value)
  );
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :expenses="expenses" :income="income" />
    <TransactionList
      :transactions="transactions"
      :deleteTransaction="DeleteTransaction"
    />
    <AddTransaction @transactionAdded="SubmitTransaction" />
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
