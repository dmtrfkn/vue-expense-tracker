<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="textInputValue" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="number" v-model="amountInputValue" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const emit = defineEmits(['transactionSubmitted']);

const textInputValue = ref('');
const amountInputValue = ref('');
const toast = useToast();

const onSubmit = () => {
  if (!textInputValue.value || !amountInputValue.value) {
    toast.error('Both field must be filled');
    return;
  }
  console.log('submit');
  emit('transactionSubmitted', {
    text: textInputValue.value,
    amount: parseFloat(amountInputValue.value),
  });
  textInputValue.value = '';
  amountInputValue.value = '';
};
// const props = defineProps({
//   text: {
//     type: String,
//     required: true,
//   },
//   amount: {
//     type: String,
//     required: true,
//   },
// });
</script>
