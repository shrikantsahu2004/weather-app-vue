<template>
  <div id="app">
    <button class="location-btn" @click="getWeatherByLocation">Use device location... <fa icon="map-marker-alt"/></button>
    <div class="search-box">
      <input type="text" id="" class="search-bar" placeholder="Enter City Name..."  @keypress="getWeatherByCountry" v-model="city"/>
    </div>
    <div class="main-container" v-if="typeof weather.main!='undefined'">
      <div class="info-container" >
        <div class="location-box">
          <div class="date">
            <Date :dateObject="new Date()" standard=true />
          </div>
          <div class="location">
            {{weather.name}}, {{weather.sys.country}} 
          </div>
        </div>
        <div class="weather-container">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="wh">
            <span class="humidity">Humidity: {{weather.main.humidity}}%</span>
            <span class="weather">
              <WeatherStatus :weatherCondition="this.weather.weather[0].main"/>
            </span>
          </div>
        </div>
      </div>

        <button id="extra-button" @click="showMoreInfo()">Show More</button>
        <div id="additional-info">
          <div class="min-temp">Min.: {{weather.main.temp_min}}°C</div>
          <div class="max-temp">Max.: {{weather.main.temp_max}}°C</div>
          <div class="pressure">Pressure: {{weather.main.pressure}} mb</div>
          <div class="sunrise">Sunrise: <Date :dateObject="new Date(weather.sys.sunrise*1000)" standard=false /></div>
          <div class="sunset">Sunset: <Date :dateObject="new Date(weather.sys.sunset*1000)" standard=false /></div>
          <div class="wind-speed">Wind: {{weather.wind.speed}} m/s</div>
        </div>

    </div>

    <div v-else-if="start"></div>
    <div v-else class="error">
      Unable to fetch Weather Data. Please Check city name and internet connectivity
    </div>
  </div>
</template>

<script>
import WeatherStatus from './components/WeatherStatus.vue'
import Date from './components/Date.vue'

export default {
  components: { WeatherStatus, Date},
  name: 'App',
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url: 'https://api.openweathermap.org/data/2.5/',
      city: '',
      weather: {},
      start: true,
      lat: 0,
      long: 0,
    }
  },
  methods: {
    async getWeatherByCountry(event) {
      if(event.key == "Enter") {
        await fetch(`${this.url}weather?q=${this.city}&units=metric&APPID=${this.api_key}`)
        .then((res) => res.json())
        .then(this.setWeather)
      }
    },
    async getWeatherByLocation() {
      if(navigator.geolocation) {
        await navigator.geolocation.getCurrentPosition(
          async position => {
            this.lat = position.coords.latitude
            this.long = position.coords.longitude
            await fetch(`${this.url}weather?lat=${this.lat}&lon=${this.long}&units=metric&APPID=${this.api_key}`)
            .then((res) => res.json())
            .then(this.setWeather)
          },
          error => {
            console.log(error.message)
          },
        )
      }
      else {
        console.log("Geolocation not supported");
      }
    },
    setWeather(data) {
      console.log(data)
      this.weather = data
      this.start = false
    },
    showMoreInfo() {
      var x = document.getElementById("additional-info");
      var y = document.getElementById("extra-button");
      if (x.style.display == "none") {
        x.style.display = "block";
        y.innerHTML = "Show Less";
      } else {
        x.style.display = "none";
        y.innerHTML = "Show More";
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 100px;
}
.temp {
  color: white;
  font-size: 4.5rem;
}
#additional-info {
  display: none;
  padding-top: 10px;
  color: rgba(255, 255, 255, 0.8);
  position: relative;
  width: 35%;
  margin: 0 auto 40px auto;
  background-color: rgba(0, 0, 0, 0.8);
  border-radius: 20px;
  padding-bottom: 20px;
  letter-spacing: 2px;
  font-size: 1rem;
}
#extra-button {
  background-color:#DB4437;
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  cursor: pointer;
  border-radius: 10px;
  font-size: 1rem;
}
#extra-button:hover {
  background-image: linear-gradient(rgba(0, 0, 0, 0.2) 0 0);
}
</style>

