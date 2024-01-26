<template>
  <div class="user__wrapper">
    <img v-if="!isNeedPlaceholder" :src="data.photo" alt="User Photo" loading="lazy" />
    <img v-else src="@/assets/photo-cover.svg" alt="User Photo Placeholder" loading="lazy" />
    <h3>{{ data.name }}</h3>
    <div class="user__description">
      <p>{{ data.position }}</p>
      <a :href="`mailto:${data.email}`">{{ data.email }}</a>
      <FormatedPhoneNumber :phone="data.phone" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import FormatedPhoneNumber from '@/components/reusable/FormatedPhoneNumber.vue'
const props = defineProps(['data'])

// Only needed for prevent abnormal displaying of photo with placeholder photo on API
const isNeedPlaceholder = ref(false)
if (props.data.photo.includes('placeholder')) {
  isNeedPlaceholder.value = true
}
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';

.user__wrapper {
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  overflow-wrap: break-word;
  width: 100%;
  h3 {
    margin: 20px 0px;
  }
  img {
    object-fit: cover;
    width: 70px;
    height: 70px;
    border-radius: 9999px;
  }
}
.user__description {
  display: flex;
  overflow-wrap: break-word;
  flex-direction: column;
}
</style>
