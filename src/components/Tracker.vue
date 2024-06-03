<script setup>
const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
  totalIncome: {
    type: Number,
    required: true,
  },
  totalExpense: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div>
    <h1 class="font-medium">Expense Tracker</h1>
    <p>Your Balance</p>
    <span class="font-semibold text-2xl">${{ total }}</span>
  </div>
  <div
    class="grid grid-cols-2 border p-2 bg-white shadow-md text-lg"
  >
    <div class="border-r text-center mt-2">
      <p>Income</p>
      <span class="text-green-600">+${{ totalIncome }}</span>
    </div>
    <div class="text-center mt-2">
      <p>Expense</p>
      <span class="text-red-600">-${{ totalExpense }}</span>
    </div>
  </div>
</template>
