<template>
  <v-app>
    <v-content>
      <home />
    </v-content>
  </v-app>
</template>

<script>
import home from './components/Home'
export default {
  name: 'App',

  components: {
    home
  },

  data: () => ({
    //
  }),
};
</script>
