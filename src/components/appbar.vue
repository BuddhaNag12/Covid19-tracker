<template>
  <nav>
 <v-card class="overflow-hidden">
    <v-app-bar tile app>
      <v-toolbar-title>Covid 19 tracker</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text >show data table</v-btn>
      <v-btn icon href="https://github.com/BuddhaNag12/Covid19-traker" target="_blank">
        <v-icon>mdi-github</v-icon>
      </v-btn>
    </v-app-bar>
  </v-card>
  </nav>
 
</template>

<script>
export default {
  name: "appbar",
  data() {
    return {
      drawer: false
    };
  }
};
</script>