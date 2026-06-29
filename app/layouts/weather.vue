<template>
  <div class="weather-layout-wrapper">
    <slot />
  </div>
</template>

<style>
/* Ginagamit ang walang 'scoped' para maapektuhan pati ang mga parent containers ng Nuxt */
html, body, #__nuxt, .v-application, .v-application__wrap {
  background: transparent !important;
  background-color: transparent !important;
}
</style>

<style scoped>
.weather-layout-wrapper {
  background-image: url('/image/background.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  
  
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  height: 100vh;
  width: 100vw;
  
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  box-sizing: border-box;
  overflow: hidden;
}
</style>
