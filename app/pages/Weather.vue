<template>
  <div class="weather-card">

    <h1 class="app-title">🌤️ Weather</h1>

    <div v-if="currentWeather" class="weather-display">

      <div class="main-weather-box">

        <h2 class="location-name">
          {{ currentWeather.location.name }},
          {{ currentWeather.location.country }}
        </h2>


        <div class="hero-weather">

          <img
            :src="`https:${currentWeather.current.condition.icon}`"
            class="weather-icon"
          />


          <div class="temp-condition">

            <span class="main-temp">
              {{ currentWeather.current.temp_c }}°C
            </span>

            <p class="condition-text">
              {{ currentWeather.current.condition.text }}
            </p>

          </div>

        </div>



        <div class="forecast-box">

          <h3>Today's Forecast</h3>


          <div class="forecast-grid">

            <div
              v-for="item in forecastItems"
              :key="item.label"
              class="forecast-item"
            >

              <p class="forecast-time">
                {{ item.label }}
              </p>


              <img
                :src="`https:${item.data.condition.icon}`"
                class="forecast-icon"
              />


              <p class="forecast-temp">
                {{ item.data.temp_c }}°C
              </p>


              <small>
                {{ item.data.condition.text }}
              </small>

            </div>

          </div>


        </div>


      </div>

    </div>



    <v-btn
      color="primary"
      class="refresh-btn"
      prepend-icon="mdi-refresh"
      @click="getWeatherData"
    >
      Refresh Weather
    </v-btn>


  </div>
</template>



<script lang="ts" setup>

//@ts-nocheck

definePageMeta({
  layout:"weather",
  middleware:"auth",
});

const currentWeather = ref(null);
const forecastItems = ref([]);
const timeSchedule = [

  { time:"06:00", label:"6 AM" },
  { time:"12:00", label:"12 NN" },
  { time:"18:00", label:"6 PM" },
  { time:"23:00", label:"11 PM" }

];

const getWeatherData = async()=>{

  try {

    const data = await $fetch(
      "http://api.weatherapi.com/v1/forecast.json?key=8276fe2ecbcf4e1cb0132726262906&q=San Fernando&days=3&aqi=no&alerts=no"
    );
    currentWeather.value = data;
    const hours =
      data.forecast.forecastday[0].hour;


    forecastItems.value =
      timeSchedule.map(item=>{

        return {

          label:item.label,

          data:
            hours.find(
              h=>h.time.includes(item.time)
            )

        };

      });

  } catch(error){

    console.log(error);
  }

};


onMounted(getWeatherData);


</script>



<style scoped>

.weather-card {

  background:rgba(122,197,226,.85);
  padding:18px 25px 25px;
  border-radius:20px;
  text-align:center;
  box-shadow:0 12px 40px rgba(0,0,0,.25);
  backdrop-filter:blur(8px);
  width:100%;
  max-width:450px;

}

.app-title {

  font-size:24px;

  color:#2c3e50;

  margin:0 0 10px;

}

.weather-display {

  display:flex;

  flex-direction:column;

}

.main-weather-box {

  background:
  linear-gradient(
    135deg,
    #4790e2,
    #006ec2
  );


  color:white;
  padding:18px;
  border-radius:16px;


}

.location-name {

  font-size:27px;
  font-weight:600;
  margin:0 0 8px;

}

.hero-weather {

  display:flex;
  justify-content:center;
  align-items:center;
  gap:18px;

}


.weather-icon {

  width:75px;
  height:75px;

}


.temp-condition {

  text-align:left;

}

.main-temp {

  font-size:40px;
  font-weight:bold;

}

.condition-text {

  margin:3px 0 0;
  font-size:15px;

}

.forecast-box {

  margin-top:15px;
  background:rgba(255,255,255,.18);
  padding:12px;
  border-radius:15px;

}

.forecast-box h3 {
  margin:0 0 10px;
  font-size:17px;

}


.forecast-grid {

  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:8px;

}

.forecast-item {

  background:white;
  color:#2c3e50;
  border-radius:12px;
  padding:8px 4px;
  min-height:115px;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;

}

.forecast-time {
  font-size:12px;
  font-weight:bold;
  margin:0;

}

.forecast-icon {
  width:40px;
  height:40px;

}


.forecast-temp {
  font-size:16px;
  font-weight:bold;
  margin:2px;

}

.forecast-item small {
  font-size:9px;

}

.refresh-btn {
  margin-top:15px;
  width:100%;
  border-radius:12px;

}


</style>