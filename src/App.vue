<template>
  <v-app>
    <v-main>
      <URLShortener/>
    </v-main>
  </v-app>
</template>

<script>
import URLShortener from './components/URLShortener.vue';

export default {
  name: 'App',

  components: {
    URLShortener
},

  data: () => ({
    //
  }),
}
</script>
