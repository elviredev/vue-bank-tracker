<template>
  <h3>Historique</h3>
  <ul id="list" class="list">
    <li
        v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}€</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
/*
  props
*/
const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

/*
  emit
*/
const emit = defineEmits(['transactionDeleted'])

/*
  transmet l'ID
  transactionDeleted sera écouté par App.vue qui déclenchera action de suppression
*/
const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}

</script>

