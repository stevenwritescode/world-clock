<template>
  <v-app>
    <v-app-bar app color="secondary" height="100">
      <div class="d-flex justify-space-between text-md-h3 text-sm-h4 text-xs-h5 font-weight-black ml-4 text-left" width="225">{{ localTime.t24h }}</div>
      <div class="d-flex justify-space-between text-md-h1 text-sm-h2 text-xs-h3 font-weight-thin mx-auto text-center">UNKNOWER</div>
      <div class="d-flex justify-space-between  text-md-h3 text-sm-h4 text-xs-h5 font-weight-black mr-4 text-right" width="225">{{ localTime.t12h }}</div>
    </v-app-bar>

    <v-main class="main">
      <WorldClock />
    </v-main>
  </v-app>
</template>

<script>
import WorldClock from "./components/WorldClock";
import moment from "moment-timezone";

export default {
  name: "App",

  components: {
    WorldClock,
  },

  data: () => ({
    localTime: { location: "Denver", t24h: moment().tz("America/Denver").format("H:mm:ss"), t12h: moment().tz("America/Denver").format("h:mm:ss") },
  }),
  mounted() {
    this.updateTime();
  },
  methods: {
    updateTime: function () {
      const objThis = this;
      setInterval(function () {
        objThis.localTime.t24h = moment().tz("America/Denver").format("HH:mm:ss");
        objThis.localTime.t12h = moment().tz("America/Denver").format("hh:mm:ss");
      }, 500);
    },
  },
};
</script>

<style scoped>
.text-h1 {
  letter-spacing: 30px;
}
</style>
