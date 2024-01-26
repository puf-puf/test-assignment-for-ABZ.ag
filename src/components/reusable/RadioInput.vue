<template>
  <div class="radio__wrapper">
    <p>Select your position</p>
    <label v-if="isError" class="input__wrapper-error-label">{{ errorText }}</label>
    <div class="radio__input" v-for="option in radioOptions" :key="option.id">
      <input @change="handleChange" type="radio" :id="option.id" :name="theme" :value="option.id" />
      <label :for="option.id">{{ option.name }}</label>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const props = defineProps(['errorText', 'onSubmitError', 'radioOptions', 'theme'])
const emit = defineEmits(['update:modelValue'])
const isError = ref(false)

function handleChange(event) {
  emit('update:modelValue', event.target.value)
}

watch(
  () => props.onSubmitError,
  (value) => {
    isError.value = value
  }
)
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';
@import '@/assets/main.scss';

.radio__wrapper {
  margin: 25px 0px 32px 0px;
  position: relative;
  @include breakpoint(sm) {
    width: 328px;
  }
  @include breakpoint(md) {
    width: 380px;
  }
  p {
    margin: 0px 0px 11px 0px;
  }
}
.radio__input {
  padding: 0px 5px;
  margin: 0px 0px 7px 0px;
  input[type='radio'] {
    &:checked,
    &:not(:checked) {
      display: none;
      + label {
        position: relative;
        padding-left: 28px;
        cursor: pointer;
        display: inline-block;
        &:before {
          content: '';
          position: absolute;
          left: 0;
          top: 2px;
          width: 20px;
          height: 20px;
          border: 1px solid #d0cfcf;
          border-radius: 999px;
        }
        &:after {
          content: '';
          width: 12px;
          height: 12px;
          background: $secondary-color;
          position: absolute;
          top: 6px;
          left: 4px;
          border-radius: 999px;
        }
      }
    }
    &:not(:checked) {
      + label::after {
        opacity: 0;
        -webkit-transform: scale(0);
        transform: scale(0);
      }
    }
    &:checked {
      + label {
        &::after {
          opacity: 1;
          -webkit-transform: scale(1);
          transform: scale(1);
        }
        &::before {
          border-color: $secondary-color;
        }
      }
    }
  }
}
.input__wrapper-error-label {
  position: absolute;
  bottom: -20px;
  left: 0px;
  color: red;
  font-size: 12px;
}
</style>
