<template>
    <div class="current-weather" :class="{ loading }">
      <div class="weather-main">
        <h1>{{ weather.name }}, {{ weather.sys.country }}</h1>
        <div class="temp-container">
          <span class="temperature" :style="tempColor">
            {{ Math.round(weather.main.temp) }}°C
          </span>
          <img 
            :src="weatherIcon" 
            :alt="weather.weather[0].description"
            class="weather-icon"
          >
        </div>
        <p class="description">
          <i :class="weatherIconClass"></i>
          {{ weather.weather[0].description }}
        </p>
        <div class="last-updated">
          Last updated: {{ formattedTime }}
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      weather: Object,
      loading: Boolean
    },
    computed: {
      weatherIcon() {
        return `https://openweathermap.org/img/wn/${this.weather.weather[0].icon}@4x.png`
      },
      weatherIconClass() {
        const iconMap = {
          '01d': 'mdi mdi-weather-sunny',
          '01n': 'mdi mdi-weather-night',
          '02d': 'mdi mdi-weather-partly-cloudy',
          '02n': 'mdi mdi-weather-night-partly-cloudy',
          '03d': 'mdi mdi-weather-cloudy',
          '03n': 'mdi mdi-weather-cloudy',
          '04d': 'mdi mdi-weather-cloudy',
          '04n': 'mdi mdi-weather-cloudy',
          '09d': 'mdi mdi-weather-pouring',
          '09n': 'mdi mdi-weather-pouring',
          '10d': 'mdi mdi-weather-rainy',
          '10n': 'mdi mdi-weather-rainy',
          '11d': 'mdi mdi-weather-lightning',
          '11n': 'mdi mdi-weather-lightning',
          '13d': 'mdi mdi-weather-snowy',
          '13n': 'mdi mdi-weather-snowy',
          '50d': 'mdi mdi-weather-fog',
          '50n': 'mdi mdi-weather-fog'
        }
        return iconMap[this.weather.weather[0].icon] || 'mdi mdi-weather-cloudy'
      },
      tempColor() {
        const temp = this.weather.main.temp
        let color = '#2196f3' // default blue
        if (temp <= 0) color = '#64b5f6' // cold
        if (temp > 25) color = '#f44336' // hot
        if (temp > 15 && temp <= 25) color = '#4CAF50' // pleasant
        return {
          background: `linear-gradient(135deg, ${color}, ${this.adjustColor(color, 20)})`
        }
      },
      formattedTime() {
        return new Date().toLocaleTimeString()
      }
    },
    methods: {
      adjustColor(color, amount) {
        return color.replace(/^#/, '').replace(/.{2}/g, x => 
          ('0' + Math.min(255, Math.max(0, parseInt(x, 16) + amount)).toString(16)).slice(-2)
        )
      }
    }
  }
  </script>
  
  <style scoped>
  .weather-icon {
    width: 100px;
    height: 100px;
    filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.3));
  }
  
  .last-updated {
    margin-top: 20px;
    font-size: 0.9em;
    color: var(--text-secondary);
  }
  </style>