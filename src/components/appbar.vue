<template>
  <nav>
 <v-card class="overflow-hidden" >
    <v-app-bar tile app color="primary">
      <v-toolbar-title class="text-uppercase white--text">Co<v-icon color="red accent-3">mdi-virus</v-icon>id19</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text color="white" href="https://github.com/BuddhaNag12/Covid19-traker" target="_blank">
      <v-icon color="white">mdi-github</v-icon>
      </v-btn>
      <v-icon @click="$vuetify.theme.dark=!$vuetify.theme.dark" v-if="$vuetify.theme.dark" >mdi-brightness-4</v-icon>
      <v-icon @click="$vuetify.theme.dark=!$vuetify.theme.dark" v-else >mdi-brightness-7</v-icon>
    </v-app-bar>
  </v-card>
  </nav>
 
</template>

<script>
export default {
  name: "appbar",

  data() {
    return {
      drawer: false,
    }
  },


};
</script>