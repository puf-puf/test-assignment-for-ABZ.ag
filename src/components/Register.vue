<template>
  <section class="register" id="register">
    <h2>Working with POST request</h2>
    <div class="register__wrapper">
      <form @submit="handleRegisterForm" class="register__form" id="register-form">
        <TextInput
          errorText="Please fill your name"
          labelText="Your name"
          :value="data.name"
          v-model="data.name"
          :onSubmitError="inputErrors.name"
          :minLength="nameMinLength"
          :maxLength="nameMaxLength"
        />
        <EmailInput
          errorText="Please fill your email"
          labelText="Email"
          :value="data.email"
          v-model="data.email"
          :pattern="emailPattern"
          :onSubmitError="inputErrors.email"
          :minLength="emailMinLength"
          :maxLength="emailMaxLength"
        />
        <PhoneInput
          :pattern="phonePattern"
          errorText="Please fill your phone"
          :onSubmitError="inputErrors.phone"
          labelText="Phone"
          :value="data.phone"
          v-model="data.phone"
          helper="+38 (XXX) XXX - XX - XX"
        />

        <RadioInput
          v-model="data.position"
          errorText="Please select your position"
          :onSubmitError="inputErrors.position"
          :radioOptions="radioOptions"
          theme="position"
        />
        <FileInput
          :value="data.file.name"
          errorText="Please select your profile picture"
          :onSubmitError="inputErrors.position"
          v-model="data.file"
        />
        <div class="submit-button">
          <MainButton
            buttonText="Sign up"
            @click="handleErrorsCheck"
            :buttonType="isSubmitAllowed ? '' : 'disabled'"
          />
        </div>
      </form>
    </div>
  </section>
</template>

<script setup>
import { onBeforeMount, ref, computed, reactive } from 'vue'
import axios from 'axios'
import MainButton from './reusable/MainButton.vue'
import TextInput from '@/components/reusable/TextInput.vue'
import EmailInput from '@/components/reusable/EmailInput.vue'

import RadioInput from '@/components/reusable/RadioInput.vue'
import FileInput from '@/components/reusable/FileInput.vue'
import PhoneInput from '@/components/reusable/PhoneInput.vue'
const radioOptions = ref([])
const emailPattern =
  /^(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])$/

const emit = defineEmits(['newUserRegistered'])
const phonePattern = '[0-9]{3}[0-9]{3}[0-9]{2}[0-9]{2}'
const emailMaxLength = 100
const emailMinLength = 2
const nameMaxLength = 60
const nameMinLength = 2
const data = ref({
  name: '',
  email: '',
  phone: '',
  position: '',
  file: ''
})
const inputErrors = reactive({
  name: false,
  email: false,
  phone: false,
  position: false,
  file: false
})
const isSubmitAllowed = computed(() => {
  return isValidData(data.value)
})

function handleErrorsCheck() {
  const keysToCheck = ['name', 'email', 'phone', 'position', 'file']
  keysToCheck.forEach((key) => {
    inputErrors[key] = data.value[key] ? false : true
  })
}

function isValidData(data) {
  if (typeof data !== 'object') return
  const objectValues = Object.values(data)
  for (let value of objectValues) {
    if (typeof value === 'object') {
      const innerValues = Object.values(value)
      for (let innerValue of innerValues) {
        if (innerValue.length <= 0) {
          return false
        }
      }
    } else if (value.length <= 0) {
      return false
    }
  }
  return true
}
const validateName = () => {
  if (data.value.name.length <= 0) return false
  if (data.value.name.length < nameMinLength) return false
  if (data.value.name.length > nameMaxLength) return false
  return true
}
const validateEmail = () => {
  if (data.value.email.length <= 0) return false
  if (!(data.value.email.match(emailPattern) !== null)) return false
  if (data.value.email.length < emailMinLength) return false
  if (data.value.email.length > emailMaxLength) return false
  return true
}
const validatePhone = () => {
  if (data.value.phone.length <= 0) return false
  if (!(data.value.phone.match(phonePattern) !== null)) return false
  if (data.value.phone.length !== 10) return false
  return true
}
const validatePosition = () => {
  if (!data.value.position) return false
  if (!radioOptions) return console.error('radioOptions missing')
  // check for selected non existing option ID
  const optionsID = []
  for (let option of radioOptions.value) {
    optionsID.push(option.id.toString())
  }
  if (!optionsID.includes(data.value.position)) return false
  return true
}
const validateFile = () => {
  if (!data.value.file) return false
  if (!data.value.file.type.includes('image')) return false
  if (data.value.file.size > 5_000_000) return false
  // Checking width and height
  const loadImage = (src) => {
    return new Promise((resolve, reject) => {
      const image = new Image()
      image.src = src

      image.onload = () => {
        resolve(image)
      }

      image.onerror = (error) => {
        reject(error)
      }
    })
  }
  try {
    const imageURL = URL.createObjectURL(data.value.file)
    const image = loadImage(imageURL)
    if (image.width < 70 || image.height < 70) {
      URL.revokeObjectURL(imageURL)
      return false
    }
    return true
  } catch (error) {
    console.error('Error loading image', error)
    return false
  }
}
const clearInputs = () => {
  const inputs = ['name', 'email', 'phone', 'position', 'file']
  inputs.forEach((key) => {
    data.value[key] = ''
  })
}
function handleRegisterForm(e) {
  e.preventDefault()
  if (!validateName()) return
  if (!validateEmail()) return
  if (!validatePhone()) return
  if (!validatePosition()) return
  if (!validateFile()) return

  const form = new FormData()
  const formattedNumber = `+38${data.value.phone}`
  form.append('name', data.value.name)
  form.append('email', data.value.email)
  form.append('phone', formattedNumber)
  form.append('position_id', data.value.position)
  form.append('photo', data.value.file)
  const token = localStorage.getItem('token')
  axios
    .post(`${import.meta.env.VITE_API_LINK}/${import.meta.env.VITE_API_VERSION}/users`, form, {
      headers: { Token: token, 'Content-Type': 'multipart/form-data' }
    })
    .then((response) => {
      if (response.data.success) {
        emit('newUserRegistered', response.data.user_id)
        clearInputs()
      }
    })
    .catch((error) => {
      console.error(error)
    })
}

onBeforeMount(() => {
  axios({
    method: 'get',
    url: `${import.meta.env.VITE_API_LINK}/${import.meta.env.VITE_API_VERSION}/positions`
  }).then(function (response) {
    radioOptions.value = response.data.positions
  })
})
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';

.register {
  max-width: 1024px;
  @include breakpoint(sm) {
    padding: 0px 16px;
  }
  @include breakpoint(md) {
    padding: 0px 32px;
  }

  @include breakpoint(lg) {
    padding: 0px 60px;
  }
  h2 {
    text-align: center;
  }
}
.register__form {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 50px 0px 100px 0px;
  .submit-button {
    text-align: center;
    margin: 50px 0px 0px 0px;
  }
}
</style>
