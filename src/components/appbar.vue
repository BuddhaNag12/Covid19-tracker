<template>
  <nav>
 <v-card class="overflow-hidden">
    <v-app-bar tile app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" class="hidden-sm-and-up"></v-app-bar-nav-icon>
      <v-toolbar-title>Covid 19 tracker</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text to="/datatable">show data table</v-btn>
      <v-btn icon href="https://github.com/BuddhaNag12/Covid19-traker" target="_blank">
        <v-icon>mdi-github</v-icon>
      </v-btn>
    </v-app-bar>
  </v-card>

    <v-navigation-drawer v-model="drawer" app bottom color="secondary">
      <v-list >
        <v-list-item>
          <v-list-item-content>
            <v-subheader class="white--text">More Options</v-subheader>
          </v-list-item-content>
          <v-spacer></v-spacer>
          <v-icon @click.stop="drawer = !drawer">mdi-close</v-icon>
        </v-list-item>
      </v-list>
      <v-divider></v-divider>
      <v-list dense >
          <v-list-item link >
            <v-btn text block class="d-flex justify-start white--text">
              about
            </v-btn>
          </v-list-item>
      </v-list>
    </v-navigation-drawer>
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