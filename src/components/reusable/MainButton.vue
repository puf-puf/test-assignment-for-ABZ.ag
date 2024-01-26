<template>
  <button :class="`${buttonTypeRef === 'disabled' ? 'disabled' : ''} main-button`">
    {{ buttonText }}
  </button>
</template>

<script setup>
import { ref, watch } from 'vue'
const props = defineProps(['buttonText', 'buttonType'])
const buttonTypeRef = ref(props.buttonType)

watch(props, (e) => {
  if (e.buttonType === '' || false) {
    buttonTypeRef.value = false
  }
})
</script>

<style lang="scss" scoped>
@import '@/assets/main.scss';
.main-button {
  background-color: $primary-color;
  padding: 6px 16px;
  border-radius: 20px;
  min-width: 100px;
  color: black;
  transition-property: color, background-color;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
.disabled {
  background-color: #b4b4b4;
  cursor: not-allowed;
  color: #fff;
}
</style>
