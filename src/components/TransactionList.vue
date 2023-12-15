<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">
      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
      >
        {{ transaction.text }}
        <span>{{
          transaction.amount < 0
            ? `- $
            ${transaction.amount.toString().slice(1)}`
            : transaction.amount
        }}</span>
        <button class="delete-btn">x</button>
      </li>
    </ul>
  </div>
</template>
<!-- Option API -->
<!-- <script>
export default {
  data() {
    return {
      transactions: [
        { id: 1, text: "Flower", amount: -19.99 },
        { id: 2, text: "Salary", amount: 299.33 },
        { id: 3, text: "Book", amount: -10 },
        { id: 4, text: "Camera", amount: 150 },
      ],
    };
  },
};
</script> -->
//
<!-- Composition API -->
<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
</script>
