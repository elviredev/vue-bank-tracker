<template>
  <h4>Votre solde</h4>
  <h1 id="balance" :class="total < 0 ? 'minus' : ''">
    {{ total }}€
  </h1>
</template>

<script setup>
/*
  props
*/
const props = defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>
