<template>
  <div>
    <Header :callApi='callApi' v-model="search"/>
    <Placeholder :films='films'/>
    
    <main id="site_main">
      <div class="container">
        <ul class="row">
          <li :key="film.id" v-for="film in films" class="col">
            <Card :base_url="base_url" :img_size="img_size" :film='film' :imgError="imgError" :Divisione="Divisione" :posterError="posterError"/>
          </li>

          <li :key="serie.id" v-for="serie in serie_tv" class="col">
            <Card :base_url="base_url" :img_size="img_size" :film='serie' :imgError="imgError" :Divisione="Divisione" :posterError="posterError"/>
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
import Header from './site_header.vue'
import Placeholder from './site_placeholder.vue'
import Card from './card_component.vue'


export default {
  name: "site_main",

  components:{
      Header,
      Placeholder,
      Card
  },

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
      } else {
        this.films = [];
        this.serie_tv = [];
      }
    },

    Divisione(numero_da_dividere) {
      let number = Math.ceil(numero_da_dividere / 2);
      return number;
    },

    imgError(event) {
      event.target.src = require("../assets/img/flag_placeholder.png");
    },

    posterError(event) {
        event.target.src = require("../assets/img/Movie_Placeholder.jpg");
    }


  }

 
};
</script>
/* /script ------------------------------------------------ */

