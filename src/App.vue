<template>
  <v-app>
    <v-app-bar app color="amber lighten-1" dark>
      <div class="d-flex align-center">
        <v-toolbar-title>
          <!-- <v-icon sharp>directions_bus</v-icon> -->
          Dublin Bus Real-time information</v-toolbar-title
        >
      </div>
      <v-spacer></v-spacer>

      <!-- <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn> -->
    </v-app-bar>

    <v-content>
      <RealTimeStops />
    </v-content>
  </v-app>
</template>

<script>
import RealTimeStops from "./components/RealTimeStops";

export default {
  name: "App",

  components: {
    RealTimeStops
  },

  data: () => ({
    //
  })
};
</script>
