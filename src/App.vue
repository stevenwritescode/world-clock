<template>
  <v-app>
    <v-app-bar app color="secondary" dark>
        <h1 class="font-weight-thin mx-auto"> UNKNOWER</h1>
    </v-app-bar>

    <v-main>
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
  letter-spacing: 30px;
  position: relative;
  right: -30px;
}
</style>