<template>
  <div :class="`input__wrapper ${isError ? 'input__wrapper-error' : ''}`">
    <input
      class="input__wrapper-floating-input"
      :aria-placeholder="labelText"
      placeholder=""
      :value="value"
      :pattern="pattern"
      type="tel"
      @change="$emit('update:modelValue', $event.target.value)"
      @focus="isError = false"
      @blur="handleCheck"
      id="phoneInput"
    />
    <small v-if="helper">{{ helper }}</small>
    <label for="phoneInput" class="input__wrapper-floating-label">{{ labelText }}</label>
    <label v-if="isError" for="phoneInput" class="input__wrapper-error-label">{{
      errorText
    }}</label>
    <label
      v-if="!isValidPhone && value.length > 0"
      for="phoneInput"
      class="input__wrapper-error-label"
      >Phone number must follow pattern</label
    >
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue'

const props = defineProps(['errorText', 'onSubmitError', 'labelText', 'value', 'pattern', 'helper'])
const isError = ref(false)
const isValidPhone = computed(() => {
  return props.value.match(props.pattern) !== null
})
watch(
  () => props.onSubmitError,
  (value) => {
    isError.value = value
  }
)
function handleCheck() {
  if (props.value.length <= 0) {
    isError.value = true
  } else {
    isError.value = false
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/main.scss';
@import '@/assets/_breakpoints.scss';
.input__wrapper {
  position: relative;
  small {
    padding: 0px 0px 0px 16px;
    color: #7e7e7e;
  }
  @include breakpoint(sm) {
    width: 328px;
  }
  @include breakpoint(md) {
    width: 380px;
  }
}
.input__wrapper-floating-input {
  border: 1px solid #d0cfcf;
  background-color: $background-color;
  padding: 14px 16px;
  font-size: 16px;
  width: 100%;
  &:focus {
    outline: none;
    ~ .input__wrapper-floating-label {
      top: -12px;
      font-size: 16px;
    }
  }
}
.input__wrapper-floating-label {
  font-size: 16px;
  position: absolute;
  pointer-events: none;
  left: 18px;
  top: 25%;
  padding: 0 5px;
  margin-left: -5px;
  background-color: $background-color;
  transition: 0.2s ease all;
  -moz-transition: 0.2s ease all;
  -webkit-transition: 0.2s ease all;
  color: #7e7e7e;
}

.input__wrapper-floating-input:not(:placeholder-shown) ~ .input__wrapper-floating-label {
  top: -12px;
  font-size: 16px;
}

.input__wrapper-error {
  .input__wrapper-floating-input {
    border: 1px solid red;
    color: red;
  }
  .input__wrapper-floating-label {
    color: red;
  }
}
.input__wrapper-error-label {
  position: absolute;
  bottom: -40px;
  left: 18px;
  color: red;
  font-size: 12px;
}
</style>
