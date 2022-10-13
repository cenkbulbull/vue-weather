<template>
    <div class="city" v-if="weather">
    <div>
      <span>{{ weather.name }} - {{ weather.sys.country }}</span>
      <span>
        <img
          :src="'http://openweathermap.org/img/wn/'+ weather.weather[0].icon +'@2x.png'"
        />
      </span>
      <span>{{ `${weather.weather[0].description.toUpperCase()}` }}</span>
      <span>Sıcaklık: {{ `${Math.round(weather.main.temp)}` }} °C</span>
      <span
        >Min: {{ `${Math.floor(weather.main.temp_min)}` }}°C Max:
        {{ `${Math.ceil(weather.main.temp_max)}` }}°C</span
      >
      <span>Nem : {{ weather.main.humidity }} %</span>
    </div>
  </div>
</template>
<script>
import { eventBus } from "../main";
export default {
  data() {
    return {
      weather: null
    };
  },
  created() {
    eventBus.$on("weather", data => {
      this.weather = data;
    });
  }
};
</script>
<style scoped>
.city {
  display: flex;
  position: absolute;
  bottom: 0;
  color: rgb(231, 230, 230);
  width: 100%;
  font-size: 15px;
  font-weight: 600;
}
.city div {
  display: flex; 
  flex-direction: row; 
  justify-content: space-around;
  align-items: center;
  backdrop-filter: blur(5px);
  width: 100%;
  height: 200px;
}
.city div span:first-child{
  background-color: #ec6e4c; 
  padding: 5px;
  border-radius: 5px;
}
@media screen and (max-width:700px) {
  .city{
    bottom: 0px;
    font-size: 13px;
  }
  .city div {
    flex-direction: column;
    height: 100%
  }
  .city div span{
    margin-bottom: 21px;
  }
}
</style>
