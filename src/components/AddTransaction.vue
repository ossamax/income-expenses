<script setup>
import { ref } from "vue";
const emit = defineEmits(["transactionAdded"]);
const text = ref("");
const amount = ref("");

function OnSubmit() {
  if (text.value && amount.value) {
    let transactionData = {
      text: text.value,
      amount: parseFloat(amount.value),
    };

    emit("transactionAdded", transactionData);
    text.value = "";
    amount.value = "";
  }
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="OnSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
