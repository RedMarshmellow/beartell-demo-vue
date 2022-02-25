<template>
  <div class="weather-container">
    <div class="weather-wrap">
      <div class="search-box">
        <input type="text" placeholder="Search..." class="search-bar"
               v-model="query" v-on:keypress="fetchWeather" />
      </div>
      <div class="weather-info" v-if="typeof weather.main!='undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}},{{weather.sys.country }}
          </div>
          <div class="date">{{ todaysDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="icon">
            <img v-bind:src="weather_icon" alt=""/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data(){
    return{
      api_key: "15b3f19133d81f3d08c0f4d62c8f5097",
      url_base: "https://api.openweathermap.org/data/2.5/",
      icon_url: "http://openweathermap.org/img/wn/",
      weather_icon: "",
      query: "",
      weather: {},
    };
  },
  methods:{
    async fetchWeather(e){
      if (e.key === "Enter"){
        let response = await axios.get(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`);
        this.setResults(response.data);
      }
    },
    setResults(response){
      this.weather = response;
      this.weather_icon = this.icon_url + response.weather[0].icon + ".png";
    },
    todaysDate(){
      let offset = (this.weather.timezone)/60;
      let month = moment().utcOffset(offset).format("MMM");
      let day =  moment().utcOffset(offset).format("ddd");
      let date =  moment().utcOffset(offset).format("D");
      let year =  moment().utcOffset(offset).format("YYYY");
      let time = moment().utcOffset((offset)).format("h:mm A");
      return `${day}, ${month} ${date} ${year}, ${time}`;
    },
  },
  name: "weatherApp"
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;500;700;900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat",serif;
}
.weather-container{
  background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSRnJGDD945WmjGYPKfhIWNTOlYBPmOS8Ai7I24o5AQGGBBYffydM_-7h4qe_fbX64bm4I&usqp=CAU");
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  width: 375px;
  border-radius: 25px;
  margin: 50px auto 0;
  box-shadow: 0 0 30px #00000065;
}
.weather-wrap {
  height: 100%;
  padding: 25px;
  border-radius: 25px;
  background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.15),
      rgba(0, 0, 0, 0.4)
  );
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: rgba(255, 255, 255, 0.5) none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  font-style: italic;
  text-align: center;
  margin-top: 30px;
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>