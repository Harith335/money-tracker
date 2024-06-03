<script setup>
import Tracker from "./components/Tracker.vue";
import Transactions from "./components/Transactions.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, onMounted, ref } from "vue";
import { toast } from "vue3-toastify";

let transactions = ref([]);

const total = computed(() =>
  transactions.value.reduce((total, number) => total + number.amount, 0)
);

const totalIncome = computed(() =>
  transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((total, number) => total + number.amount, 0)
);

const totalExpense = computed(() =>
  transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((total, number) => total + number.amount, 0)
);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const handleTransactionSubmitted = (transactiondata) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactiondata.text,
    amount: transactiondata.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success("transaction add.");
};

const handleTransactionDelete = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

const generateUniqueId = () => Math.floor(Math.random() * 1000);
</script>

<template>
  <div class="flex flex-col items-center min-h-screen bg-slate-100">
    <div class="max-w-[480px] mt-8">
      <Tracker
        :total="+total"
        :totalExpense="+totalExpense"
        :totalIncome="+totalIncome"
      />
      <Transactions
        :transactions="transactions"
        @transactionDelete="handleTransactionDelete"
      />
      <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
  </div>
</template>
