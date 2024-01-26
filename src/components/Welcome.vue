<template>
  <section class="welcome">
    <img
      srcset="
        @/assets/welcome-picture_360.webp   360w,
        @/assets/welcome-picture_768.webp   768w,
        @/assets/welcome-picture_1024.webp 1024w
      "
      sizes="(max-width: 360px) 320px,
         (max-width: 768px) 728px,
         (max-width: 1024px) 1014px"
      src="@/assets/welcome-picture_1024.webp"
      alt="Picture from Pexels that demonstrates blue sky and field of wheat"
    />
    <div class="welcome__wrapper">
      <div class="welcome__text">
        <h1>Test assignment for front-end developer</h1>
        <p>
          What defines a good front-end developer is one that has skilled knowledge of HTML, CSS, JS
          with a vast understanding of User design thinking as they'll be building web interfaces
          with accessibility in mind. They should also be excited to elarn, as the world of
          Front-End Development keeps evolving
        </p>
        <MainButton @click="signUpClick" buttonText="Sign up" />
      </div>
    </div>
  </section>
</template>

<script setup>
import MainButton from './reusable/MainButton.vue'
const signUpClick = () => {
  document.getElementById('register').scrollIntoView({ behavior: 'smooth' })
}
</script>

<style lang="scss" scoped>
@import '@/assets/_breakpoints.scss';

.welcome {
  position: relative;
  max-width: 1024px;
  img {
    width: 100%;
    height: 630px;
    object-fit: cover;
    object-position: center;
  }
}
.welcome__wrapper::before {
  background-color: rgba($color: #000000, $alpha: 0.3);
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 99%;
}
.welcome__text {
  color: white;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  h1,
  p {
    margin: 20px 0px;
  }

  @include breakpoint(sm) {
    width: 328px;
  }
  @include breakpoint(md) {
    width: 380px;
  }
}
</style>
