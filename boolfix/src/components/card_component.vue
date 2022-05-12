<template>
    <div class="card">
              <div class="image">
                <img
                  :src="`${base_url}${img_size}${film.poster_path}`"
                  alt=""
                  @error='posterError'
                />
              </div>
              <div class="informazioni">
                <div class="titolo"><span>Titolo:</span> {{ film.title }}</div>
                <div
                  class="titolo_originale"
                  v-if="film.title !== film.original_title"
                >
                  <span>Titolo Originale:</span> {{ film.original_title }}
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
                <div class="overview">
                  <span>Overview: </span>{{ film.overview }}
                </div>
                <!-- /.overview -->
              </div>
              <!-- /.informazioni -->
            </div>
            <!-- /.card -->
</template>

<script>
export default {
    name: 'card_component',

    props:['base_url','img_size','film','imgError','Divisione','posterError']
}
</script>