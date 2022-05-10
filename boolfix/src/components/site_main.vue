<template>
  <div>
    <header>
      <input type="text" v-model="search" @keyup.enter="callApi" />
      <button @click="callApi">cerca</button>
    </header>

    <ul>
      <li :key="film.id" v-for="film in films">
        <div class="titolo">
          {{ film.title }}
        </div>
        <div class="titolo_originale">
          {{ film.original_title }}
        </div>
        <div class="lingua">
          <img
            :src="`https://countryflagsapi.com/png/${film.original_language}`"
            alt=""
            @error="imgError"
          />
          <div class="text">{{ film.original_language }}</div>
        </div>
        <div class="voto">
          {{ film.vote_average }}
        </div>
      </li>

      <li :key="serie.id" v-for="serie in serie_tv">
        <div class="titolo">
          {{ serie.name }}
        </div>
        <div class="titolo_originale">
          {{ serie.original_name }}
        </div>
        <div class="lingua">
          <img
            :src="`https://countryflagsapi.com/png/${serie.original_language}`"
            alt=""
            @error="imgError"
          />
          <div class="text">{{ serie.original_language }}</div>
        </div>
        <div class="voto">
          {{ serie.vote_average }}
        </div>
      </li>
    </ul>
  </div>
</template>
/* /template --------------------------------------------*/

<script>
import axios from "axios";

export default {
  name: "site_main",

  data() {
    return {
      films: [],
      serie_tv: [],
      search: "",
    };
  },

  methods: {
    callId(array_originale, nuovo_array) {
      for (let i = 0; i < array_originale.length; i++) {
        axios
          .get(
            `https://api.themoviedb.org/3/movie/${array_originale[i].id}?api_key=588a2090952cc5fe4696e9b998b79992`
          )
          .then((oggetto) => {
            nuovo_array.push(oggetto.data);
          })
          .catch((error) => {
            console.log(error);
            nuovo_array.push(array_originale[i]);
          });
      }
    },

    callApi() {
      if (this.search !== "") {
        /* films */
        axios
          .get(
            `https://api.themoviedb.org/3/search/movie?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`
          )
          .then((oggetto) => {
              this.films=[]
            this.callId(oggetto.data.results, this.films);
          });
        /* serie_tv */
        axios
          .get(
            `https://api.themoviedb.org/3/search/tv?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`
          )
          .then((oggetto) => {
              this.serie_tv = []
            console.log(oggetto.data.results);
            this.callId(oggetto.data.results, this.serie_tv);
          });
      }
    },

    imgError(event) {
      event.target.src = require("../assets/img/flag_placeholder.png");
    },
  },

  mounted() {},
};
</script>
/* /script ------------------------------------------------ */

<style lang="scss">
</style>
/* /style ------------------------------------------------- */