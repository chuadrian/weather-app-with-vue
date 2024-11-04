<template>
    <div class="weather-details">
      <div class="detail-item" v-for="(detail, index) in details" :key="index">
        <i :class="detail.icon"></i>
        <div class="detail-info">
          <span class="detail-label">{{ detail.label }}</span>
          <span class="detail-value">{{ detail.value }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      weather: Object
    },
    computed: {
      details() {
        return [
          {
            icon: 'mdi mdi-thermometer',
            label: 'Feels like',
            value: `${Math.round(this.weather.main.feels_like)}°C`
          },
          {
            icon: 'mdi mdi-water',
            label: 'Humidity',
            value: `${this.weather.main.humidity}%`
          },
          {
            icon: 'mdi mdi-weather-windy',
            label: 'Wind Speed',
            value: `${Math.round(this.weather.wind.speed * 3.6)} km/h`
          },
          {
            icon: 'mdi mdi-gauge',
            label: 'Pressure',
            value: `${this.weather.main.pressure} hPa`
          },
          {
            icon: 'mdi mdi-weather-sunset-up',
            label: 'Sunrise',
            value: this.formatTime(this.weather.sys.sunrise)
          },
          {
            icon: 'mdi mdi-weather-sunset-down',
            label: 'Sunset',
            value: this.formatTime(this.weather.sys.sunset)
          }
        ]
      }
    },
    methods: {
      formatTime(timestamp) {
        return new Date(timestamp * 1000).toLocaleTimeString([], {
          hour: '2-digit',
          minute: '2-digit'
        })
      }
    }
  }
  </script>
  
  <style scoped>
  .detail-info {
    display: flex;
    flex-direction: column;
  }
  
  .detail-label {
    font-size: 0.9em;
    color: var(--text-secondary);
  }
  
  .detail-value {
    font-size: 1.2em;
    font-weight: 600;
  }
  </style>