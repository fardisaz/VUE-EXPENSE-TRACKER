<template>
  <div>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="text">Text</label>
        <input
          type="text"
          id="text"
          placeholder="Enter text..."
          v-model="text"
        />
      </div>
      <div class="form-control">
        <label for="amount"
          >Amount<br />
          (negative - expense, positive - income)
        </label>
        <input
          type="number"
          id="amount"
          placeholder="Enter amount..."
          v-model="amount"
        />
      </div>
      <button class="btn">Add transaction</button>
    </form>
  </div>
</template>
<script setup>
import { defineEmits, ref } from "vue";
import { useToast } from "vue-toastification";
const text = ref("");
const amount = ref(null);
const emit = defineEmits(["transactionSubmitted"]);
// For the validation of text and number we use a lib called Vue Toastification
const toast = useToast();
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled");
    return;
  }
  emit("transactionSubmitted", { text: text.value, amount: amount.value });
  text.value = "";
  amount.value = null;
};
</script>
