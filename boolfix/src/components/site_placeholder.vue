<template>
  <div class="placeholder" v-if="films.length === 0">
    <h2>NON SONO PRESENTI FILM O SERIE TV DA MOSTRARE</h2>
    <h3>Utilizza la barra di ricerca in alto</h3>
  </div>
  <!-- /.placeholder -->
</template>

<script>
export default {
  name: "site_placeholder",

  props:['films']
};
</script>