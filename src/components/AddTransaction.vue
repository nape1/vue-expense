<template>
  <section class="bottom-fixed">
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="text">Text</label>
        <input
          ref="inputRef"
          type="text"
          id="text"
          placeholder="Enter text..."
          v-model="text"
        />
      </div>
      <div class="form-control">
        <label for="amount"
          >Amount <br />
          (negative - expense, positive - income)</label
        >
        <input type="text" id="amount" placeholder="Enter amount..." v-model.number="amount" />
      </div>
      <button class="btn">Add transaction</button>
    </form>
  </section>
</template>

<script setup>
import { useToast } from 'vue-toastification'
import { ref } from 'vue'

const text = ref('')
const amount = ref('')
const inputRef = ref(null)

// Get toast interface
const toast = useToast()

const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () => {
  if (!text.value || !amount.value) {
    // Display a toast error message if either field is empty
    toast.error('Both fields must be filled.')
    return
  }
  if (typeof amount.value !== 'number') {
    toast.error('Invalid Amount')
    return
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  }

  emit('transactionSubmitted', transactionData)

  // Clear form fields
  text.value = ''
  amount.value = ''
  inputRef.value.focus()
}
</script>

<style scoped>
.bottom-fixed {
  position: fixed;
  width: 380px;
  bottom: 0;
}
</style>
