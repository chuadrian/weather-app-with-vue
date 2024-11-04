<template>
    <div class="forecast-panel">
      <h2>5-Day Forecast</h2>
      <div class="forecast-items">
        <div 
          v-for="item in forecast" 
          :key="item.dt" 
          class="forecast-item"
          :class="{ 'is-today': isToday(item.dt) }"
        >
          <div class="forecast-date">{{ formatDate(item.dt) }}</div>
          <img 
            :src="getIcon(item.weather[0].icon)" 
            :alt="item.weather[0].description"
            class="forecast-icon"
          >
          <div class="forecast-temp-range">
            <span class="max-temp">{{ Math.round(item.main.temp_max) }}°</span>
            <span class="min-temp">{{ Math.round(item.main.temp_min) }}°</span>
          </div>
          <div class="forecast-description">
            {{ item.weather[0].description }}
          </div>
          <div class="forecast-details">
            <span>
              <i class="mdi mdi-water"></i>
              {{ item.main.humidity }}%
            </span>
            <span>
              <i class="mdi mdi-weather-windy"></i>
              {{ Math.round(item.wind.speed * 3.6) }} km/h
            </span>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      forecast: Array
    },
    methods: {
      formatDate(timestamp) {
        return new Date(timestamp * 1000).toLocaleDateString('en-US', {
          weekday: 'short',
          month: 'short',
          day: 'numeric'
        })
      },
      getIcon(icon) {
        return `https://openweathermap.org/img/wn/${icon}@2x.png`
      },
      isToday(timestamp) {
        const date = new Date(timestamp * 1000)
        const today = new Date()
        return date.toDateString() === today.toDateString()
      }
    }
  }
  </script>
  
  <style scoped>
  .forecast-icon {
    width: 64px;
    height: 64px;
    margin: 10px 0;
  }
  
  .forecast-temp-range {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin: 10px 0;
  }
  
  .max-temp {
    font-weight: 700;
    color: var(--text-primary);
  }
  
  .min-temp {
    color: var(--text-secondary);
  }
  
  .forecast-description {
    font-size: 0.9em;
    color: var(--text-secondary);
    margin: 5px 0;
    text-transform: capitalize;
  }
  
  .forecast-details {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 10px;
    font-size: 0.9em;
    color: var(--text-secondary);
  }
  
  .forecast-details i {
    font-size: 1.1em;
    margin-right: 4px;
  }
  
  .is-today {
    border: 2px solid var(--primary-color);
    background: rgba(33, 150, 243, 0.1);
  }
  </style>