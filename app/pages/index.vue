<template>
  <div>
    <h1>Hello World</h1>
    <v-btn>Submit</v-btn>
    <v-btn @click="logout">Logout</v-btn>

    <h2 class="text -h5 font-weight-bold mb-1">{{ user?.name }} </h2>
    <p class="text-body-2 text-grey-darken-1"> {{ user?.email }}</p>

  </div>
</template>


<script lang="ts" setup>

definePageMeta({
  middleware: 'auth'
})

const logout = () => {
  localStorage.removeItem('google_user')
  localStorage.removeItem('google_token')
  window.google?.accounts.id.disableAutoSelect()
  navigateTo('/login')

}
const user = ref<any>(null)
const showScanner = ref(false)
const scannedValue = ref('')
onMounted(() => {
  const savedUser = localStorage.getItem('google_user')

  if (!savedUser) {
    navigateTo('/login')
    return
  }

  user.value = JSON.parse(savedUser)

}
)



</script>

<style></style>