<template>
  <header id="site_header">
    <div class="container">
      <div class="logo">
        <div>BOOLFLIX</div>
      </div>
      <!-- /.logo -->
      <div class="search">
        <input
          type="text"
          placeholder="Cerca"
          v-bind:value="value"
          v-on:input="$emit('input', $event.target.value)"
          @keyup.enter="callApi"
        />
        <button @click="callApi">cerca</button>
      </div>
      <!-- /.search -->
    </div>
  </header>
  <!-- /#site_header -->
</template>

<script>
export default {
  name: "site_header",

  props: ["callApi",'value'],
};
</script>