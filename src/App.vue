<template>
  <div class="app">
    <div class="weather-card">
      <h1>Weather Dashboard</h1>

      <div class="search">
        <input
          type="text"
          placeholder="Enter city name"
          v-model="cityInput"
          @keyup.enter="getWeather"
        />
        <button @click="getWeather">Search</button>
      </div>

      <p v-if="weatherStore.currentWeather" class="location">
        Showing weather for {{ weatherStore.currentWeather.name }}
      </p>

      <div v-if="weatherStore.currentWeather" class="current-weather">
        <h2>{{ weatherStore.currentWeather.name }}</h2>
        <p class="description">
          {{ weatherStore.currentWeather.weather[0].description }}
        </p>
        <p class="temp">
          {{ weatherStore.currentWeather.main.temp }} &deg;C
        </p>
      </div>

      <div v-if="weatherStore.forecast.length" class="forecast">
        <h3>5-Day Forecast</h3>
        <ul>
          <li
            v-for="(day, index) in weatherStore.forecast"
            :key="index"
          >
            <span>{{ day.dt_txt }}</span>
            <span>{{ day.main.temp }} &deg;C</span>
            <span>{{ day.weather[0].description }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useWeatherStore } from '@/stores/weather'

const cityInput = ref('')
const weatherStore = useWeatherStore()

function getWeather() {
  if (cityInput.value.trim() !== '') {
    weatherStore.fetchWeather(cityInput.value)
    cityInput.value = ''
  }
}
</script>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #74ebd5, #acb6e5);
  font-family: "Segoe UI", Tahoma, sans-serif;
}

.weather-card {
  background: white;
  width: 420px;
  padding: 2.5rem;
  border-radius: 18px;
  text-align: center;
  box-shadow: 0 20px 45px rgba(0, 0, 0, 0.15);
}

h1 {
  margin-bottom: 1.2rem;
}

.search {
  display: flex;
  gap: 0.6rem;
  margin-bottom: 1rem;
}

input {
  flex: 1;
  padding: 0.6rem;
  font-size: 15px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

button {
  padding: 0.6rem 1rem;
  border: none;
  border-radius: 8px;
  background: #4f46e5;
  color: white;
  font-size: 15px;
  cursor: pointer;
}

button:hover {
  background: #4338ca;
}

.location {
  color: #666;
  font-size: 14px;
}

.current-weather {
  margin-top: 1.5rem;
}

.description {
  text-transform: capitalize;
  color: #555;
}

.temp {
  font-size: 2.8rem;
  font-weight: bold;
  margin-top: 0.5rem;
}

.forecast {
  margin-top: 2rem;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  padding: 0.6rem 0;
  border-bottom: 1px solid #eee;
}

li span:last-child {
  text-transform: capitalize;
}
</style>
