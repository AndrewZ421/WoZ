<template>
  <v-app>
    <v-main>
      <Conversion/>
    </v-main>
  </v-app>
</template>

<script>
import Conversion from './components/Conversion.vue';

export default {
  name: 'App',

  components: {
    Conversion,
  },

  data: () => ({
    //
  }),
};
</script>
