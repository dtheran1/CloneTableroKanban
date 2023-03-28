<script setup>
import { ref, defineEmits } from 'vue';

const text = ref('')

const emits = defineEmits(['onNewItem'])

const handleSubmit = () => {
  if (!text.value) return

  emits('onNewItem', text.value)
  text.value = ''
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" v-model="text">
  </form>
</template>

<style scoped>
input {
  background: transparent;
  color: wheat;
  height: 40px;
}
</style>