<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <RevenusDepenses :revenus="+revenus" :depenses="+depenses" />
    <TransactionsList
        :transactions="transactions"
        @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"  />
  </div>
</template>

<script setup>
/*
  imports
*/
  import Header from "@/components/Header.vue";
  import Balance from "@/components/Balance.vue";
  import RevenusDepenses from "@/components/RevenusDepenses.vue";
  import TransactionsList from "@/components/TransactionsList.vue";
  import AddTransaction from "@/components/AddTransaction.vue";
  import { ref, computed, onMounted } from "vue";
  import { useToast } from "vue-toastification";


/*
  toast
*/
const toast = useToast()

/*
  transactions
*/
// datas
const transactions = ref([
  // { id: 1, text: 'Fleurs', amount: -19.99 },
  // { id: 2, text: 'Prime', amount: 299.97 },
  // { id: 3, text: 'Livre', amount: -10 },
  // { id: 4, text: 'Camera', amount: 150 }
])

// add transaction
const handleTransactionSubmitted = (transactionData) => {
  // push les datas dans le tab transactions - créer un id
  transactions.value.push({
    id: generateUniqueID(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  // save to LS
  saveTransactionsToLocalStorage()
  // message - toast
  toast.success('Transaction ajoutée.')
}

// Unique ID
const generateUniqueID = () => {
  return Math.floor(Math.random() * 1000000)
}

// delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => id !== transaction.id)
  // save to LS
  saveTransactionsToLocalStorage()
  // Message toast
  toast.success('Transaction supprimée')
}

/*
  solde - total
*/
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0).toFixed(2)
})

/*
  total revenus
*/
const revenus = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc +transaction.amount
      }, 0).toFixed(2)
})

/*
  total depenses
*/
const depenses = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc +transaction.amount
      }, 0).toFixed(2)
})

/*
  Local Storage
*/
onMounted(() => {
  // vérifier le Local Storage
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

// Save to LS
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>