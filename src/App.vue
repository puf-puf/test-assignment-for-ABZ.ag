<script setup>
import Register from './components/Register.vue'
import Users from './components/Users.vue'
import Welcome from './components/Welcome.vue'
import Header from './components/Header.vue'
import { onBeforeMount, ref } from 'vue'
import axios from 'axios'
onBeforeMount(() => {
  const tokenExpiresAt = localStorage.getItem('tokenExpiresAt')
  if (!tokenExpiresAt) {
    fetchToken()
    return
  }
  if (Date.now() > tokenExpiresAt) {
    fetchToken()
    return
  }

  function fetchToken() {
    axios({
      method: 'get',
      url: `${import.meta.env.VITE_API_LINK}/${import.meta.env.VITE_API_VERSION}/token`
    }).then(function (response) {
      localStorage.setItem('token', response.data.token)
      localStorage.setItem('tokenExpiresAt', Date.now() + 2400000)
    })
  }
})
const isNewUser = ref(false)
</script>

<template>
  <Header />

  <main>
    <Welcome />
    <Users :isNewUser="isNewUser" />
    <Register @newUserRegistered="() => (isNewUser = true)" />
  </main>
</template>

<style scoped></style>
