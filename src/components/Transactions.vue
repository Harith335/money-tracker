<script setup>
import { ref, onMounted } from "vue";
import { useDateFormat, now } from "@vueuse/core";

const formatDate = ref("");

onMounted(() => {
  formatDate.value = useDateFormat(now(), "YYYY-MM-DD").value;
});

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["transactionDelete"]);

const deleteTransaction = (id) => {
  emit("transactionDelete", id);
};
</script>

<template>
  <h2 class="mt-8 border-b-2 mb-4">History</h2>
  <div>
    <ul
      class="grid my-1"
      v-for="transaction in transactions"
      :key="transaction.id"
    >
      <li
        class="flex flex-initial justify-between bg-white p-1 border-r-4 relative group"
        :class="transaction.amount > 0 ? 'border-green-600' : 'border-red-600'"
      >
        <button
          class="absolute -left-7 top-0 bg-red-600 text-white px-2 py-4 rounded-s opacity-0 transition-opacity duration-300 ease-in-out group-hover:opacity-100"
          @click="deleteTransaction(transaction.id)"
        >
          X
        </button>
        <div class="grid">
          <span>{{ transaction.text }}</span>
          <span>{{ formatDate }}</span>
        </div>
        <span>${{ transaction.amount }}</span>
      </li>
    </ul>
  </div>
</template>
