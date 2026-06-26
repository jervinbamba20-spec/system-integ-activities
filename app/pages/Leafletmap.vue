<template>
  <div ref="mapContainer" class="map"></div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

const mapContainer = ref<HTMLElement | null>(null);

onMounted(async () => {
  if (!mapContainer.value) return;

  const leaflet = await import("leaflet");
  const L = leaflet.default;

  const map = L.map(mapContainer.value).setView([0, 0], 2);

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OpenStreetMap contributors",
  }).addTo(map);

  navigator.geolocation.getCurrentPosition(
    (position) => {
      const lat = position.coords.latitude;
      const lng = position.coords.longitude;

      map.setView([lat, lng], 18);

      L.marker([lat, lng])
        .addTo(map)
        .bindPopup("📍 Your Current Location")
        .openPopup();

      L.circle([lat, lng], {
        radius: position.coords.accuracy,
        color: "#2563eb",
        fillOpacity: 0.2,
      }).addTo(map);
    },
    (error) => {
      console.error(error);
      alert("Unable to get your location.");
    },
    {
      enableHighAccuracy: true,
      timeout: 15000,
      maximumAge: 0,
    }
  );
});
</script>

<style scoped>
.map {
  width: 100%;
  height: 500px;
  border-radius: 10px;
}
</style>