<script>
import axios from "axios";

export default {
  name: "App",
  data: () => ({
    api_url: "https://api.openweathermap.org/data/2.5/",
    api_key: "814ca0274a5978ef78c6b9b44ff87547",
    query: "Amol",
    weather: {},
    showBookmarks: false,
  }),

  methods: {
    fetchWeather() {
      axios
        .get(
          `${this.api_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
        .then((response) => (this.weather = response.data))
        .catch((error) => console.log(error));
    },
  },
  computed: {
    getDate: function () {
      const d = new Date();
      const monthNames = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const month = d.getMonth() + 1;
      const weekday = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ][d.getDay()];
      const date = `${d.getDate()}, ${monthNames[month]} ${d.getFullYear()}`;
      return { date, weekday };
    },
  },
  mounted() {
    this.fetchWeather();
  },
};
</script>

<template>
  <div class="container" v-if="typeof weather.main != 'undefined'">
    <div class="weather-side">
      <div class="weather-gradient"></div>
      <div class="date-container">
        <h2 class="date-dayname">{{ getDate.weekday }}</h2>
        <span class="date-day">{{ getDate.date }}</span>
        <span class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </span>
      </div>

      <div class="weather-container">
        <i class="weather-icon far fa-moon" />
        <h1 class="weather-temp">{{ Math.round(weather.main.temp) }}°C</h1>
        <h3 class="weather-desc">{{ weather.weather[0].main }}</h3>
      </div>
    </div>

    <div class="info-side">
      <div class="today-info-container">
        <div class="today-info">
          <div class="humidity">
            <span class="title">HUMIDITY</span
            ><span class="value">{{ weather.main.humidity }} %</span>
            <div class="clear"></div>
          </div>
          <div class="wind">
            <span class="title">WIND</span
            ><span class="value">{{ weather.wind.speed }} km/h</span>
            <div class="clear"></div>
          </div>
        </div>
      </div>

      <div class="location-container">
        <form @submit.prevent="fetchWeather()" accept-charset="utf-8">
          <input
            class="location-input"
            type="text"
            v-model="query"
            placholder="Enter your location"
          />
          <button type="submit" class="location-button">
            <i class="location-icon fas fa-map-marker-alt"></i>
            <span>Change your location</span>
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap");

:root {
  --gradient: linear-gradient(135deg, #72edf2 10%, #5151e5 100%);
}

* {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  line-height: 1.25em;
}

.clear {
  clear: both;
}

body {
  margin: 0;
  width: 100%;
  height: 100%;
  background: #3A3E59;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
}

.container {
  border-radius: 15px;
  margin-top: 10%;
  color: #ffffff;
  height: 400px;
}

.weather-side {
  -webkit-transition: -webkit-transform 200ms ease;
  transition: -webkit-transform 200ms ease;
  -o-transition: transform 200ms ease;
  transition: transform 200ms ease;
  transition: transform 200ms ease, -webkit-transform 200ms ease;
  position: relative;
  height: 100%;
  border-radius: 15px;
  width: 100%;
  -webkit-box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  float: left;
}
.weather-side:hover {
  transform: scale(0.95);
  transition: all 0.3s;
}
.weather-gradient {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px,
    rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
  background: #ED6B5B;
  border-radius: 15px;
  opacity: 0.8;
}

* {
  font-family: "Montserrat", sans-serif;
  color: #F9AC66;
}

.date-container {
  position: absolute;
  top: 25px;
  left: 25px;
}

.date-dayname {
  margin: 0;
}

.date-day {
  display: block;
}

.location {
  display: inline-block;
  margin-top: 10px;
}

.location-icon {
  display: inline-block;
  font-size: 1.2em;
  margin-right: 7px;
}

.weather-container {
  position: absolute;
  bottom: 25px;
  left: 25px;
}

.weather-icon {
  font-size: 65px;
}

.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 4.2em;
}

.weather-desc {
  margin: 0;
}

.info-side {
  position: relative;
  float: left;
  height: 100%;
  padding-top: 25px;
  min-width: 330px;
}

.today-info {
  padding: 15px;
  margin: 0 25px 25px 25px;
  border-radius: 10px;
}

.today-info > div:not(:last-child) {
  margin: 0 0 10px 0;
}

.today-info > div .title {
  float: left;
  font-weight: 700;
}

.today-info > div .value {
  float: right;
}

.week-list {
  list-style-type: none;
  padding: 0;
  margin: 10px 35px;
  -webkit-box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
}

.week-list > li {
  float: left;
  padding: 15px;
  transition: 200ms ease;
  border-radius: 10px;
}

.week-list > li.active {
  background: #fff;
  color: #222831;
  border-radius: 10px;
}

.week-list > li .day-name {
  display: block;
  margin: 10px 0 0 0;
  text-align: center;
}

.week-list > li .day-icon {
  display: block;
  height: 30px;
  width: auto;
  margin: 0 auto;
}

.week-list > li .day-temp {
  display: block;
  text-align: center;
  margin: 10px 0 0 0;
  font-weight: 700;
}

.location-container {
  padding: 25px 35px;
}

.location-button {
  outline: none;
  width: 100%;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  border: none;
  border-radius: 15px;
  padding: 15px;
  font-family: "Montserrat", sans-serif;
  background: #C36B84;
  color: #F9AC66;
  font-weight: 700;
  -webkit-box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  -webkit-transition: -webkit-transform 200ms ease;
  transition: -webkit-transform 200ms ease;
  -o-transition: transform 200ms ease;
  transition: transform 200ms ease;
  transition: transform 200ms ease, -webkit-transform 200ms ease;
}

.location-input {
  outline: none;
  width: 100%;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  border: none;
  border-radius: 15px;
  padding: 15px 20px;
  font-family: "Montserrat", sans-serif;
  color: #C36B84;
  background: none;
  font-weight: 700;
  border: 2px solid rgba(#F9AC66, 0.5);
  margin-bottom: 15px;
}

.location-input:focus {
  -moz-transition: all 0.3s ease-in-out;
  -o-transition: all 0.3s ease-in-out;
  -webkit-transition: all 0.3s ease-in-out;
  transition: all 0.3s ease-in-out;
  border-color: rgba(rgb(255, 146, 43), 0.8);
}

.location-button:hover {
  -webkit-transform: scale(0.95);
  -ms-transform: scale(0.95);
  transform: scale(0.95);
}

.location-button .feather {
  height: 1em;
  width: auto;
  margin-right: 5px;
}

@media (max-width: 760px) {
  .weather-gradient {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    border-radius: 15px;
    opacity: 0.8;
  }

  .weather-side {
    margin: 80px 7.75% 0;
    height: 80%;
    width: 90%;
    float: left;
    -webkit-box-shadow: none;
    box-shadow: none;
    -webkit-transform: none;
    transform: none;
  }

  .container {
    margin: 0;
    box-shadow: none;
    background: transparent;
    height: 430px;
    width: 95%;
  }
}
</style>
