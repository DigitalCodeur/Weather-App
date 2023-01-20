<script setup>
</script>

<template>
  <header class="py-1 font-serif bg-gradient-to-r from-zinc-900 to-zinc-600">
    <nav class="container grid-cols-2 mx-auto sm:grid">
      <div class="flex w-3/4 mx-auto text-3xl sm:mx-0 text-sky-100 sm:w-1/2">
        <span class="text-6xl text-white"><font-awesome-icon icon="fa-cloud" /></span>
        <span class="my-auto"> Weather App</span>
      </div>
      <form method="get" v-on:submit.prevent="getWeather"
        class="flex my-2 ml-auto rounded-lg shadow-lg bg-slate-100 w-96 h-2/3 sm:my-auto">
        <input type="search" v-model="citySearch"
          class="w-full px-2 py-3 text-lg text-black rounded-l-lg bg-inherit focus:outline-none sm:py-0"
          placeholder="Entrer une ville">
        <button type="submit" class="px-3 text-xl text-green-700 rounded-r-md">
          <font-awesome-icon icon="fa-solid fa-magnifying-glass" />
        </button>
      </form>
    </nav>
  </header>


  <main class="font-serif lg:pt-20 sm:pt-52 bg-img">
    <div v-if="error">
      <p class="text-2xl text-center text-red-700">Aucune ville trouvée</p>
    </div>

    <div
      class="container grid-cols-2 px-10 py-16 mx-auto text-white shadow-xl sm:grid shadow-gray-900 bg-glass rounded-2xl">

      <section class="mx-10">
        <div class="flex justify-between">
          <p class="text-2xl">{{ weather.date }}</p>
          <p class="text-2xl">{{ weather.cityName }}, {{ weather.country }}</p>
        </div>
        <div class="flex ml-20 mt-14">
          <img :src="icon + weather.icon + '@2x.png'" class="my-auto w-36" v-if="weather.temperature" alt="">
          <p class="my-auto ml-5 text-7xl">{{ weather.temperature }}&deg;C</p>
        </div>
        <p class="text-2xl text-center mb-14 mt-7"> <em>{{ weather.description }}</em> </p>

      </section>
      <section class="mx-10 mt-20 sm:mt-0">
        <p class="w-16 mx-auto mb-10 text-4xl border-b-4">Détails</p>
        <div class="mt-5">
          <div class="flex justify-between mt-3">
            <p class="my-auto text-2xl">temperature Maximale:</p>
            <p class="my-auto text-3xl">{{ weather.temperatureMax }}&deg;C</p>
          </div>
          <div class="flex justify-between mt-3">
            <p class="my-auto text-2xl">temperature Minimale:</p>
            <p class="my-auto text-3xl">{{ weather.temperatureMin }}&deg;C</p>
          </div>
          <div class="flex justify-between mt-3">
            <p class="my-auto text-2xl">Sensation de:</p>
            <p class="my-auto text-3xl">{{ weather.feelsLike }}&deg;C</p>
          </div>
          <div class="flex justify-between mt-3">
            <p class="my-auto text-2xl">Humidité: </p>
            <p class="my-auto text-3xl">{{ weather.humidity }}%</p>
          </div>
          <div class="flex justify-between mt-3">
            <p class="my-auto text-2xl">Visibilité: </p>
            <p class="my-auto text-3xl">{{ weather.visibility }} km</p>
          </div>
        </div>

      </section>
    </div>
  </main>



</template>



<script>

import axios from 'axios'

export default {

  data() {
    return {

      citySearch: "london",
      error: false,
      icon: "http://openweathermap.org/img/wn/",
      weather: [],

    }
  },

  methods: {

    getWeather: function () {
      const key = "5952f97a35c986e11c8deae41ac55fe0";
      axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&lang=fr&units=metric`)
        .then(reponse => {
          // console.log(reponse.data);
          this.weather.cityName = reponse.data.name;
          this.weather.country = reponse.data.sys.country;
          this.weather.temperature = Math.round(reponse.data.main.temp);
          this.weather.temperatureMax = Math.round(reponse.data.main.temp_max);
          this.weather.temperatureMin = Math.round(reponse.data.main.temp_min);
          this.weather.feelsLike = Math.round(reponse.data.main.feels_like);
          this.weather.description = reponse.data.weather[0].description;
          this.weather.humidity = reponse.data.main.humidity;
          this.weather.visibility = (reponse.data.visibility) / 1000;
          const date = new Date();
          let dateLocale = date.toLocaleString('fr-FR', {
            year: 'numeric',
            month: 'long',
            day: 'numeric',

          });
          this.weather.date = dateLocale;
          this.weather.icon = reponse.data.weather[0].icon;
          this.error = false;

        })
        .catch(error => {
          this.error = true;
        })
    },

  },

  mounted() {

    this.getWeather();

  },

}
</script>


<style scoped>

</style>
