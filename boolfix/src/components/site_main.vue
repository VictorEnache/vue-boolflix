<template>
  <div>
    <header>
      <input type="text" v-model="search" @keyup.enter="callApi" />
      <button @click="callApi">cerca</button>
    </header>

    <ul>
      <li :key="film.id" v-for="film in films">
        <div class="image">
            <img :src="`${base_url}${img_size}${film.poster_path}`" alt="">
        </div>
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
          <div class="image">
              <img :src="`${base_url}${img_size}${serie.poster_path}`" alt="">
          </div>
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
      base_url: 'https://image.tmdb.org/t/p/',
      img_size: 'w200'
    };
  },

  methods: {
    callId(array_originale, nuovo_array,prefisso_url) {
      for (let i = 0; i < array_originale.length; i++) {
        axios
          .get(
            `${prefisso_url}${array_originale[i].id}?api_key=588a2090952cc5fe4696e9b998b79992`
          )
          .then((oggetto) => {
            nuovo_array.push(oggetto.data);
          })
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
            this.callId(oggetto.data.results, this.films,'https://api.themoviedb.org/3/movie/');
          });
        /* serie_tv */
        axios
          .get(
            `https://api.themoviedb.org/3/search/tv?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`
          )
          .then((oggetto) => {
              this.serie_tv = []
            console.log(oggetto.data.results);
            this.callId(oggetto.data.results, this.serie_tv,'https://api.themoviedb.org/3/tv/');
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