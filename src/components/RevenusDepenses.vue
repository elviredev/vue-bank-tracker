<template>
  <div class="inc-exp-container">
    <div>
      <h4>Revenus</h4>
      <p id="money-plus" class="money plus">{{ revenus }}€</p>
    </div>
    <div>
      <h4>Dépenses</h4>
      <p id="money-minus" class="money minus">{{ depenses }}€</p>
    </div>
  </div>
</template>

<script setup>
/*
  props
 */
const props = defineProps({
  revenus: {
    type: Number,
    required: true
  },
  depenses: {
    type: Number,
    required: true
  },

})
</script>

