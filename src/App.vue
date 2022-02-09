<template>
  <v-app>
    <v-app-bar app color="secondary" height="extended">
      <h1 class="font-weight-thin mx-auto text-center">UNKNOWER</h1>
    </v-app-bar>

    <v-main class="main">
      <WorldClock />
    </v-main>
  </v-app>
</template>

<script>
import WorldClock from "./components/WorldClock";

export default {
  name: "App",

  components: {
    WorldClock,
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
h1 {
  font-size: 48pt;
  letter-spacing: 30px;
  padding-left: 30px;
}
main {
  margin-top: 150px;
}
</style>
