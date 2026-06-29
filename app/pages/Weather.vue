<template>
  <div>
    <h1>Weather Page</h1>
    <h2 v-if="currentWeather">{{ currentWeather?.current?.temp_c }} °C</h2>
    <v-btn @click="getWeatherData">
      Get Weather Data
    </v-btn>
  </div>
</template>




<script lang="ts" setup>
definePageMeta({
  layout: "weather",
  middleware: "auth",
})

const currentWeather = ref<any>(null)

const getWeatherData = async () => {
  try {
    const data = await $fetch("http://api.weatherapi.com/v1/forecast.json?key=f89070bdec6f4f27af134757262906&q=Manila&days=7&aqi=no&alerts=no")

    currentWeather.value = data
  } catch (error) {
    console.error("Failed to fetch weather data:", error)
  }
}

onMounted(() => {
  getWeatherData()
})
</script>