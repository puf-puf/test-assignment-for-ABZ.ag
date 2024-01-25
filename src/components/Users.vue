<template>
  <section class="users">
    <h2>Working with GET request</h2>
    <div class="users__wrapper">
      <UserSquare v-for="user in users" :key="user.id" :data="user" />
    </div>
    <MainButton
      v-if="currentPage !== totalPages"
      buttonText="Show more"
      @click="() => fetchUsers(currentPage + 1)"
    />
  </section>
</template>

<script setup>
import { onBeforeMount, ref, watch } from 'vue'
import axios from 'axios'
import UserSquare from '@/components/reusable/UserSquare.vue'
import MainButton from '@/components/reusable/MainButton.vue'
const users = ref([])
const currentPage = ref(1)
const usersPerPage = 6
let totalPages = ref(null)
const props = defineProps(['isNewUser'])

watch(
  () => props.isNewUser,
  async (value) => {
    if (value && users.value.length > usersPerPage) {
      users.value = []
      await fetchUsers(1)
      const usersBox = document.getElementsByClassName('users__wrapper')
      usersBox[0].scrollIntoView({ behavior: 'smooth' })
    }
  }
)
function fetchUsers(page) {
  if (page > totalPages.value && totalPages.value !== null) return
  axios({
    method: 'get',
    url: `${import.meta.env.VITE_API_LINK}/${import.meta.env.VITE_API_VERSION}/users`,
    params: { page: page, count: usersPerPage }
  })
    .then(function (response) {
      if (users.value.length === 0) {
        users.value = response.data.users
        totalPages.value = response.data.total_pages
      } else {
        users.value = users.value.concat(response.data.users)
        currentPage.value++
      }
    })
    .catch(function (error) {
      console.error(error)
    })
}
onBeforeMount(() => {
  fetchUsers(1)
})
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';
.users__wrapper {
  display: grid;
  gap: 20px;
  margin: 50px 0px;
  justify-content: center;
  @include breakpoint(sm) {
    grid-template-columns: 328px;
  }
  @include breakpoint(md) {
    grid-template-columns: repeat(2, 344px);
  }
  @include breakpoint(lg) {
    grid-template-columns: repeat(3, 328px);
  }
}
.users {
  text-align: center;
  margin: 140px 0px;
  max-width: 1024px;
  @include breakpoint(sm) {
    padding: 0px 16px;
  }
  @include breakpoint(md) {
    padding: 0px 32px;
  }
  @include breakpoint(lg) {
    padding: 0px 0px;
  }
}
</style>
