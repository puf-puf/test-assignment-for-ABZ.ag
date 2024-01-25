<template>
  <div class="file-input__wrapper">
    <label
      :class="`file__upload-button ${
        isError || isSizeError || isWeightError ? 'file__upload-button-error' : ''
      }`"
      for="photoFile"
      >Upload</label
    >
    <input
      @change="handleFileSelect"
      type="file"
      id="photoFile"
      placeholder="Upload your photo"
      accept=".png,.jpg, .jpeg"
      si
    />
    <label v-if="value" class="file__upload-placeholder" for="photoFile">{{ value }}</label>
    <label v-else class="file__upload-placeholder" for="photoFile">Upload your photo</label>
    <label v-if="isError" class="input__wrapper-error-label">{{ errorText }}</label>
    <label v-if="isWeightError" class="input__wrapper-error-label"
      >Your File weight is larger than 5 MB</label
    >
    <label v-if="isSizeError" class="input__wrapper-error-label"
      >File size must be larger 70x70 pixels</label
    >
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const props = defineProps(['value', 'onSubmitError', 'errorText'])
const isError = ref(false)
const isWeightError = ref(false)
const isSizeError = ref(false)
const emit = defineEmits(['update:modelValue'])

function handleFileSelect(e) {
  const photo = e.target.files[0]
  if (!photo) return
  if (!photo.type.includes('image/')) return
  // Checking for Weight
  if (photo.size > 5_000_000) {
    isError.value = false
    isSizeError.value = false
    isWeightError.value = true
    return
  }
  // Checking for Width
  const image = new Image()
  image.src = URL.createObjectURL(photo)
  image.onload = function () {
    if (image.width < 70 || image.height < 70) {
      URL.revokeObjectURL(photo)
      isError.value = false
      isSizeError.value = true
      isWeightError.value = false
      return
    }
  }
  emit('update:modelValue', photo)
  isError.value = false
  isSizeError.value = false
  isWeightError.value = false
}

watch(
  () => props.onSubmitError,
  (value) => {
    isError.value = value
    isWeightError.value = false
  }
)
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';
.file-input__wrapper {
  display: flex;
  position: relative;
  @include breakpoint(sm) {
    width: 328px;
  }
  @include breakpoint(md) {
    width: 380px;
  }
  input[type='file'] {
    display: none;
  }
  .file__upload-button {
    border: 1px solid black;
    border-radius: 4px 0px 0px 4px;
    padding: 14px 15px;
  }
  .file__upload-button-error {
    border-color: red;
    color: red;
  }
  .file__upload-placeholder {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    height: 56px;
    width: 300px;
    border-left: none;
    border: 1px solid lightgray;
    padding: 14px 16px;
    border-radius: 0px 4px 4px 0px;
  }
}
.input__wrapper-error-label {
  position: absolute;
  bottom: -25px;
  left: 0px;
  color: red;
  font-size: 12px;
}
</style>
