<template>
  <header class="header">
    <div class="header__wrapper">
      <img src="../assets/logo.svg" alt="Test Task logo icon" />
      <div class="header__buttons">
        <MainButton @click="usersClick" buttonText="Users" />
        <MainButton @click="signUpClick" buttonText="Sign up" />
      </div>
    </div>
  </header>
</template>

<script setup>
import MainButton from './reusable/MainButton.vue'
const signUpClick = () => {
  document.getElementById('register').scrollIntoView({ block: 'start', behavior: 'smooth' })
}
const usersClick = () => {
  document.getElementById('users').scrollIntoView({ behavior: 'smooth' })
}
</script>

<style lang="scss" scoped>
@import '@/assets/main.scss';
@import '@/assets/_breakpoints.scss';

.header__wrapper {
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  @include breakpoint(sm) {
    padding: 13px 16px;
  }
  @include breakpoint(md) {
    padding: 12px 32px;
  }
  @include breakpoint(lg) {
    max-width: 1024px;
    padding: 12px 60px;
  }
}
.header__buttons {
  display: flex;
  gap: 10px;
}
.header {
  width: 100%;
  background-color: #fff;
  display: flex;
  justify-content: center;
}
</style>
