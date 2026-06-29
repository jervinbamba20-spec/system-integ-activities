<template>
  <v-container fluid class="pa-5">

    <v-card class="pa-4">

      <h2 class="mb-4">My Current Location</h2>

      <div ref="mapRef" class="map"></div>

      <v-btn
        class="mt-4"
        color="primary"
        prepend-icon="mdi-arrow-left"
        @click="goBack"
      >
        Back
      </v-btn>

    </v-card>

  </v-container>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue"
import L from "leaflet"

definePageMeta({
  middleware: "auth"
})

const mapRef = ref<HTMLDivElement | null>(null)

const goBack = () => {
  navigateTo("/")
}

onMounted(() => {
  if (!mapRef.value) return

  
  const map = L.map(mapRef.value).setView([15.4827, 120.5979], 13)

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map)

  
  L.marker([15.4827, 120.5979])
    .addTo(map)
    .bindPopup("My Location")
    .openPopup()

  setTimeout(() => {
    map.invalidateSize()
  }, 300)
})
</script>

<style scoped>
.map {
  height: 450px;
  width: 100%;
  border-radius: 8px;
}
</style>