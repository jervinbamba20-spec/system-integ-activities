<template>
  <div ref="mapContainer" class="map"></div>
  
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue"

const mapContainer = ref()

onMounted(async () => {
  const leaflet = await import("leaflet")
  const L = leaflet.default
  const map = L.map(mapContainer.value).setView([0, 0], 2)

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OpenStreetMap contributors",
  }).addTo(map)

  navigator.geolocation.getCurrentPosition((position) => {
    const lat = position.coords.latitude
    const lng = position.coords.longitude

    map.setView([lat, lng], 17)

    L.marker([lat, lng])
      .addTo(map)
      .bindPopup("📍 Your Current Location")
      .openPopup()

    L.circle([lat, lng], {
      radius: position.coords.accuracy,
      color: "#1976D2",
      fillOpacity: 0.2,
    }).addTo(map)
  })
})
</script>

<style scoped>
.map {
  width: 100%;
  height: 500px;
  border-radius: 10px;
}
</style>