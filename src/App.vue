<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : '' ">
    <div class="container">
      <div class="search-box">
        <input type="text" placeholder="Search" class="search-bar" v-model="query" @keyup.enter="fetchWeather"></div>
        <div class="weather-wrap" v-if="weather.main">
          <div class="location-box">
            <div class="date">{{dateBuilder()}}</div>
            <div class="location">{{weather.name}},{{weather.sys.country}}</div>
          </div>
          <div class="weather-box">
            <img :src="icon_url" >
            <p class="temperature">{{Math.round(weather.main.temp)}}°C</p>
            <p class="feelsLike">體感 <br>
            {{Math.round(weather.main.feels_like)}}°C</p>

          </div>
          <div class="weather-description">{{weather.weather[0].description}}</div>
        </div>
    </div>
  </div>
</template>

<script>
// First we get the viewport height and we multiple it by 1% to get a value for a vh unit
const vh = window.innerHeight * 0.01
// Then we set the value in the --vh custom property to the root of the document
document.documentElement.style.setProperty('--vh', `${vh}px`)

export default {
  name: 'App',
  data () {
    return {
      api_key: '3ae2ddee592f59218a601e06df3a1abd',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      icon_base: 'http://openweathermap.org/img/wn/',
      icon_url: '',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      }
    },
    setResults (results) {
      this.weather = results
      this.icon_url = `${this.icon_base}${this.weather.weather[0].icon}@2x.png`
    },
    dateBuilder () {
      const d = new Date()
      const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      const day = days[d.getDay()]
      const date = d.getDate()
      const month = months[d.getMonth()]
      const year = d.getFullYear()
      return `${day}, ${date} ${month}, ${year}`
    }
  }

}

</script>

<style lang="scss">
@import "../node_modules/scss-reset/_reset.scss";
@import "./assets/scss/style.scss";
</style>
