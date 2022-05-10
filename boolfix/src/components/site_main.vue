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
      films: "",
      tvSeries: "",
      search: "",
    };
  },

  methods: {
    callApi() {
      if (this.search !== "") {
        axios
        .get(
        `https://api.themoviedb.org/3/search/movie?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`
        )
        .then((oggetto) => {
        this.films = oggetto.data.results;
        console.log(oggetto.data.results);
        });

        axios
        .get(
            `https://api.themoviedb.org/3/search/tv?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`)
        .then((oggetto) => {
        this.tvSeries = oggetto.data.results;
        console.log(oggetto.data.results);
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