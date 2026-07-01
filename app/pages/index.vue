<template>
  <v-container fluid class="dashboard-bg fill-height">
    <v-row justify="center" align="center" class="fill-height">
      <v-col cols="12" sm="8" md="5" lg="4">
        <v-card rounded="xl" elevation="12" class="pa-6 text-center">

          <v-avatar size="100" class="mb-4">
            <v-img :src="user?.picture" alt="Profile" />
          </v-avatar>

          <h1 class="text-h4 font-weight-bold mb-1">
            Welcome Jervin Cutie
          </h1>

          <div class="text-h6">
            {{ user?.name }}
          </div>

          <div class="text-body-2 text-grey-darken-1 mb-6">
            {{ user?.email }}
          </div>

          <v-divider class="mb-6" />

          <!-- QR BUTTON -->
          <v-btn
            color="primary"
            prepend-icon="mdi-qrcode-scan"
            block
            class="mb-3"
            @click="openQr"
          >
            Open QR Scanner
          </v-btn>

          <!-- MAP BUTTON -->
          <v-btn
            color="success"
            prepend-icon="mdi-map"
            block
            class="mb-3"
            @click="openMap"
          >
            Open Map
          </v-btn>

          <!-- WEATHER BUTTON -->
          <v-btn
            color="info"
            prepend-icon="mdi-weather-partly-cloudy"
            block
            class="mb-3"
            @click="openWeather"
          >
            Check Weather
          </v-btn>

          <!-- LOGOUT BUTTON -->
          <v-btn
            color="error"
            prepend-icon="mdi-logout"
            block
            @click="logout"
          >
            Logout
          </v-btn>

        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>


<script setup lang="ts">

definePageMeta({
  middleware: "auth"
})


const user = ref<any>(null)


onMounted(() => {
  const savedUser = localStorage.getItem("google_user")

  if (!savedUser) {
    navigateTo("/login")
    return
  }

  user.value = JSON.parse(savedUser)
})


const openQr = () => {
  navigateTo("/qr")
}


const openMap = () => {
  navigateTo("/mymap")
}


// WEATHER FUNCTION
const openWeather = () => {
  navigateTo("/weather")
}


const logout = () => {

  localStorage.removeItem("google_user")
  localStorage.removeItem("google_token")

  window.google?.accounts.id.disableAutoSelect()

  navigateTo("/login")
}

</script>


<style scoped>

.dashboard-bg {
  background: white;
  min-height: 100vh;
}


.fill-height {
  min-height: 100vh;
}

</style>