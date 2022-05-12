<template>
  <div class="card">
    <div class="image">
      <img
        :src="`${base_url}${img_size}${film.poster_path}`"
        alt=""
        @error="posterError"
      />
    </div>
    <div class="informazioni">
      <div class="titolo"><span>Titolo:</span> {{ title }}</div>
      <div class="titolo_originale" v-if="film.title !== film.original_title">
        <span>Titolo Originale:</span> {{ original_title }}
      </div>
      <div class="lingua">
        <div class="lingua_image">
          <img
            :src="`https://countryflagsapi.com/png/${film.original_language}`"
            alt=""
            @error="imgError"
          />
        </div>
        <div class="text">{{ film.original_language }}</div>
      </div>
      <div class="voto">
        <!-- {{ Divisione(film.vote_average) }} -->
        <span>Voto: </span>
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
      <div class="overview"><span>Overview: </span>{{ film.overview }}</div>
      <!-- /.overview -->
    </div>
    <!-- /.informazioni -->
  </div>
  <!-- /.card -->
</template>

<script>
export default {
  name: "card_component",

  data() {
    return {
      base_url: "https://image.tmdb.org/t/p/",
      img_size: "w200",
    };
  },

  methods:{
        imgError(event) {
      event.target.src = require("../assets/img/flag_placeholder.png");
    },

        Divisione(numero_da_dividere) {
      let number = Math.ceil(numero_da_dividere / 2);
      return number;
    },


    posterError(event) {
        event.target.src = require("../assets/img/Movie_Placeholder.jpg");
    }
  },

  props: [
    "film",
    
    "title",
    "original_title",
  ],
};
</script>

