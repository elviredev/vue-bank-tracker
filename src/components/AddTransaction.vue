<template>
  <h3>Ajouter une transaction</h3>
  <form id="form" @submit.prevent="handleSubmitForm">
    <div class="form-control">
      <label for="text">Texte</label>
      <input type="text" v-model="text" id="text" placeholder="Entrer texte...">
    </div>
    <div class="form-control">
      <label for="amount">
        Montant <br>
        <span class="amount-infos">(ex. -15 pour dépense, 15 pour revenu)</span>
      </label>
      <input type="text" v-model="amount" id="amount" placeholder="Entrer montant...">
    </div>
    <button class="btn">Ajouter transaction</button>
  </form>
</template>

<script setup>
/*
  imports
*/
import { ref } from "vue";
import { useToast } from "vue-toastification";

/*
  ref
*/
const text = ref('')
const amount = ref('')

/*
  toast
*/
const toast = useToast()

/*
  emit
*/
const emit = defineEmits(['transactionSubmitted'])

/*
  submit form
*/
const handleSubmitForm = () => {
  if (!text.value || !amount.value) {
    toast.error('Les 2 champs doivent être remplis')
    return
  }

  // datas à transmettre
  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  // émettre les datas au state dans App.vue via l'event personnalisé
  emit('transactionSubmitted', transactionData)

  // reset les champs
  text.value = ''
  amount.value = ''
}

</script>

