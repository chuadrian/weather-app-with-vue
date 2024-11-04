<template>
    <div class="weather-container">
      <nav-bar 
        @search="handleSearch" 
        :loading="loading"
      ></nav-bar>
      
      <div v-if="error" class="error-message">
        <i class="mdi mdi-alert"></i>
        {{ error }}
      </div>
  
      <transition-group name="fade" tag="main" class="main-content">
        <current-weather 
          v-if="weather" 
          :weather="weather"
          :loading="loading"
          key="current"
        ></current-weather>
        
        <weather-details 
          v-if="weather" 
          :weather="weather"
          key="details"
        ></weather-details>
        
        <forecast-panel 
          v-if="forecast" 
          :forecast="forecast"
          key="forecast"
        ></forecast-panel>
      </transition-group>
    </div>
  </template>
  
  <script>
  import NavBar from './NavBar.vue'
  import CurrentWeather from './CurrentWeather.vue'
  import WeatherDetails from './WeatherDetails.vue'
  import ForecastPanel from './ForecastPanel.vue'
  import { config } from '../config'
  
  export default {
    components: {
      NavBar,
      CurrentWeather,
      WeatherDetails,
      ForecastPanel
    },
    data() {
      return {
        weather: null,
        forecast: null,
        loading: false,
        error: null,
        lastSearched: null
      }
    },
    methods: {
      async handleSearch(city) {
        if (this.loading) return
        
        this.loading = true
        this.error = null
        
        try {
          const [weatherResponse, forecastResponse] = await Promise.all([
            fetch(`${config.BASE_URL}/weather?q=${city}&appid=${config.API_KEY}&units=metric`),
            fetch(`${config.BASE_URL}/forecast?q=${city}&appid=${config.API_KEY}&units=metric`)
          ])
  
          if (!weatherResponse.ok || !forecastResponse.ok) {
            throw new Error('City not found')
          }
  
          const [weatherData, forecastData] = await Promise.all([
            weatherResponse.json(),
            forecastResponse.json()
          ])
  
          this.weather = weatherData
          this.forecast = this.processForecast(forecastData)
          this.lastSearched = city
          
          
          this.saveToRecent(city)
        } catch (err) {
          this.error = err.message === 'City not found' 
            ? 'City not found. Please try another location.'
            : 'Failed to fetch weather data. Please try again.'
        } finally {
          this.loading = false
        }
      },
      processForecast(data) {
        return data.list.filter((item, index) => index % 8 === 0)
      },
      saveToRecent(city) {
        const recent = JSON.parse(localStorage.getItem('recentSearches') || '[]')
        if (!recent.includes(city)) {
          recent.unshift(city)
          if (recent.length > 5) recent.pop()
          localStorage.setItem('recentSearches', JSON.stringify(recent))
        }
      }
    },
    mounted() {
      
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          async (position) => {
            const { latitude, longitude } = position.coords
            try {
              const response = await fetch(
                `${config.BASE_URL}/weather?lat=${latitude}&lon=${longitude}&appid=${config.API_KEY}&units=metric`
              )
              const data = await response.json()
              if (data.name) {
                this.handleSearch(data.name)
              }
            } catch {
              this.handleSearch('London') 
            }
          },
          () => this.handleSearch('London') 
        )
      } else {
        this.handleSearch('London')
      }
    }
  }
  </script>
  
  <style scoped>
  .fade-enter-active,
  .fade-leave-active {
    transition: all 0.3s ease;
  }
  
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
    transform: translateY(20px);
  }
  </style>