<template>
  <div>
    <header id="site_header">
      <input type="text" v-model="search" @keyup.enter="callApi" />
      <button @click="callApi">cerca</button>
    </header>
    <!-- /#site_header -->

    <main id="site_main">
      <div class="container">
        <ul class="row">
          <li :key="film.id" v-for="film in films" class="col">
            <div class="card">
              <div class="image">
                <img
                  :src="`${base_url}${img_size}${film.poster_path}`"
                  alt=""
                />
              </div>
              <div class="informazioni">
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
                  <!-- {{ Divisione(film.vote_average) }} -->
                  <font-awesome-icon
                    v-for="number in 5"
                    :key="number"
                    :icon="
                      number <= Divisione(film.vote_average)
                        ? ['fas', 'star']
                        : ['far', 'star']
                    "
                  />
                </div>
              </div>
              <!-- /.informazioni -->
            </div>
            <!-- /.card -->
          </li>

          <li :key="serie.id" v-for="serie in serie_tv" class="col">
            <div class="card">
              <div class="image">
                <img
                  :src="`${base_url}${img_size}${serie.poster_path}`"
                  alt=""
                />
              </div>
              <div class="informazioni">
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
                  <font-awesome-icon
                    v-for="number in 5"
                    :key="number"
                    :icon="
                      number <= Divisione(serie.vote_average)
                        ? ['fas', 'star']
                        : ['far', 'star']
                    "
                  />
                </div>
              </div>
              <!-- /.informazioni -->
            </div>
            <!-- /.card -->
          </li>
        </ul>
      </div>
      <!-- /.container -->
    </main>
    <!-- /#site_main -->
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
      base_url: "https://image.tmdb.org/t/p/",
      img_size: "w200",
    };
  },

  methods: {
    callId(array_originale, nuovo_array, prefisso_url) {
      for (let i = 0; i < array_originale.length; i++) {
        axios
          .get(
            `${prefisso_url}${array_originale[i].id}?api_key=588a2090952cc5fe4696e9b998b79992`
          )
          .then((oggetto) => {
            nuovo_array.push(oggetto.data);
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
            this.films = [];
            this.callId(
              oggetto.data.results,
              this.films,
              "https://api.themoviedb.org/3/movie/"
            );
          });
        /* serie_tv */
        axios
          .get(
            `https://api.themoviedb.org/3/search/tv?api_key=588a2090952cc5fe4696e9b998b79992&query=${this.search}`
          )
          .then((oggetto) => {
            this.serie_tv = [];
            console.log(oggetto.data.results);
            this.callId(
              oggetto.data.results,
              this.serie_tv,
              "https://api.themoviedb.org/3/tv/"
            );
          });
      }
    },

    Divisione(numero_da_dividere) {
      let number = Math.ceil(numero_da_dividere / 2);
      return number;
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