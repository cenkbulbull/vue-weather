<template>
  <div class="input">
    <input
      v-on:keyup.enter="searchCity()"
      v-model="cityName"
      type="text"
      class="form-control mx-auto bg-transparent text-light"
      placeholder="Şehir/İlçe Girin"
    />
    <button
      @click="searchCity()"
      type="button"
      :disabled="inputDisabled"
      class="btn btn-outline-light"
    >
      Ara
    </button>
  </div>
</template>
<script>
import { eventBus } from "../main";
import { url, key } from "../assets/js/api";

export default {
  data() {
    return {
      cityName: null,
      inputDisabled: true
    };
  },
  methods: {
    searchCity(e) {
      const query = `${url}weather?q=${this.cityName}&appid=${key}&units=metric&lang=tr`;
          fetch(query)
        .then(response => {
          return response.json();
        })
        .then(weather => {
          console.log(weather);
          //this.$emit("weather",weather)
          if (weather.cod == 404) {
            alert("Böyle bir şehir bulunamadı!!");
          } else {
            eventBus.$emit("weather", weather);
          }
          this.cityName=""
        });
    }
  },
  watch: {
    cityName() {
      if (this.cityName.length != 0) {
        this.inputDisabled = false;
      } else {
        this.inputDisabled = true;
      }
    }
  }
};
</script>
<style scoped>
.input {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.input input {
  backdrop-filter: blur(15px);
  margin-top: 70px !important;
  margin-bottom: 20px;
  width: 300px;
}
@media screen and (max-width:700px) {
  .input input {
    margin-top:0px !important;
    margin-bottom: 10px;
  }
}
</style>
