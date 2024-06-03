<script setup>
import { ref } from "vue";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

const text = ref("");
const amount = ref("");

const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled.");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", transactionData);

  text.value = "";
  amount.value = "";
};
</script>

<template>
  <h2 class="mt-2 border-b-2">Add new transaction</h2>
  <form @submit.prevent="onSubmit">
    <div class="grid gap-4">
      <div class="grid gap-2 rounded">
        <label>Text</label>
        <input
          type="text"
          placeholder="Enter text.."
          class="p-2 outline-none"
          v-model="text"
        />
      </div>
      <div class="flex flex-col gap-2 rounded">
        <label>
          Amount <br />
          (negative - expense, positive - income)</label
        >
        <input
          type="text"
          placeholder="Enter amount.."
          class="p-2 outline-none"
          v-model="amount"
        />
      </div>
    </div>
    <button class="w-full bg-violet-600 mt-4 p-2 text-white">
      Add transaction
    </button>
  </form>
</template>
