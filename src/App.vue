<template>
  <div id="app">
    <div class="main-weather" v-if="weather">
      <div>
        <span>{{weather.name}}</span>
        <span><img
          :src="'http://openweathermap.org/img/wn/'+ weather.weather[0].icon +'@2x.png'"
        /></span>
        <span>{{ `${weather.weather[0].description.toUpperCase()}` }}</span>
      <span>Sıcaklık: {{ `${Math.round(weather.main.temp)}` }} °C</span>
      </div>
    </div>
    <app-input></app-input>
    <app-weather></app-weather>
  </div>
</template>

<script>
import Input from "./components/Input"
import Weather from "./components/Weather";
import { eventBus } from "./main";
import { url, key } from "./assets/js/api";



export default {
  components : {
    appInput: Input,
    appWeather : Weather
  },
  data(){
    return{
      weather:null
    }
  },
  created(){
      const query = `${url}weather?q=istanbul&appid=${key}&units=metric&lang=tr`;
      fetch(query)
        .then(response => {
          return response.json();
        })
        .then(weather => {
          //console.log(weather);
          this.weather = weather
        });
  }
}
</script>

<style>
  @import "./assets/css/main.css";
  .main-weather{
    width: 150px;
    height: 200px;
    margin: 20px auto;
  }
  .main-weather div{
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    width: 150px;
    height: 200px;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 5px 5px 20px #000;
    background-image: url(./assets/images/istanbul.jpg);
    background-size: cover;
    background-position: center;
    background-repeat: no-repeats;
    /*background-color: transparent;*/
    backdrop-filter: blur(15px);
    color: #fff;
  }
  .main-weather div span:first-child{ 
    background-color:#ec6e4c; 
    padding: 3px;
    margin-top: 4px;
    text-align: center;
    border-radius: 5px;
  }
  
  @media screen and (max-width:700px) {
   .main-weather{
    margin: 10px auto;
    width: 120px;
    height: 190px;
  }
  .main-weather div{
    font-size: 13px;
    width: 120px;
    height: 190px;
    padding: 10px;
  }
}
</style>
