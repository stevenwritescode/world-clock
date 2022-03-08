<template>
  <v-container fluid>
    <v-row class="text-center" align="center">
      <v-col class="date col-md-4 d-none d-md-flex">
        <v-card class="pa-8 mx-auto flex-grow-1 vertical-align">
          <div class="text-md-h5 text-lg-h3 font-weight-bold">TODAY</div>
          <div class="text-sm-h4 font-weight-bold">{{ today }}</div>
          <div class="text-sm-h4 font-weight-bold">{{ todayYear }}</div>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col class="col-lg-4 col-xs-12">
        <v-card class="pa-6 mx-auto">
          <h2>{{ mainTime.location }}</h2>
          <h1>{{ mainTime.time }}</h1>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col class="date col-md-4 d-none d-md-flex">
        <v-card class="pa-8 mx-auto flex-grow-1">
          <div class="text-md-h5 text-lg-h3 font-weight-bold">TOMORROW</div>
          <div class="text-sm-h4 font-weight-bold">{{ tomorrow }}</div>
          <div class="text-sm-h4 font-weight-bold">{{ tomorrowYear }}</div>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="3" v-for="(tz, i) in displayedRow1" :key="i">
        <v-card class="py-8">
          <div class="text-h2 font-weight-bold text-center text-uppercase">{{ tz.location }}</div>
          <div class="text-h2 font-weight-bold text-center ma-4">{{ tz.t24h }}</div>
          <div class="text-h4 font-weight-bold text-center">{{ tz.t12h }}</div>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="3" v-for="(tz, i) in displayedRow2" :key="i">
        <v-card class="py-8">
          <div class="text-h2 font-weight-bold text-center text-uppercase">{{ tz.location }}</div>
          <div class="text-h2 font-weight-bold text-center ma-4">{{ tz.t24h }}</div>
          <div class="text-h4 font-weight-bold text-center">{{ tz.t12h }}</div>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="text-center" v-if="nextShow">
      <v-col cols="12">
        <div class="text-md-h3 text-sm-h4 mt-4 next">Next {{ nextShow.type }} {{ nextShow.fromNow }} at {{ nextShow.displayedTime }}</div>
        <div class="text-md-h4 text-sm-h5 mt-4 next">{{ nextShow.name }}</div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import moment from "moment-timezone";
export default {
  name: "WorldClock",

  data: () => ({
    showList: [],
    rowIteration: 1,
    displayedRow1: [],
    displayedRow2: [],
    alwaysDarkMode: true,
    timeInterval: "",
    today: moment().tz("America/Denver").format("MMMM Do"),
    todayYear: moment().tz("America/Denver").format("YYYY"),
    tomorrow: moment().tz("America/Denver").add(1, "day").format("MMMM Do"),
    tomorrowYear: moment().tz("America/Denver").add(1, "day").format("YYYY"),

    mainTime: { location: "UTC", time: moment().tz("UTC").format("H:mm:ss") },
    rows: [
      [
        { location: "Los Angeles", t24h: moment().tz("America/Los_Angeles").format("H:mm:ss"), t12h: moment().tz("America/Los_Angeles").format("h:mm:ss A") },
        { location: "Denver", t24h: moment().tz("America/Denver").format("H:mm:ss"), t12h: moment().tz("America/Denver").format("h:mm:ss A") },
        { location: "Houston", t24h: moment().tz("US/Central").format("H:mm:ss"), t12h: moment().tz("US/Central").format("h:mm:ss A") },
        { location: "New York", t24h: moment().tz("America/New_York").format("H:mm:ss"), t12h: moment().tz("America/New_York").format("h:mm:ss A") },
      ],
      [
        { location: "Hawaii", t24h: moment().tz("US/Hawaii").format("H:mm:ss"), t12h: moment().tz("US/Hawaii").format("h:mm:ss A") },
        { location: "Alaska", t24h: moment().tz("US/Alaska").format("H:mm:ss"), t12h: moment().tz("US/Alaska").format("h:mm:ss A") },
        { location: "Houston", t24h: moment().tz("US/Central").format("H:mm:ss"), t12h: moment().tz("US/Central").format("h:mm:ss A") },
        { location: "New York", t24h: moment().tz("America/New_York").format("H:mm:ss"), t12h: moment().tz("America/New_York").format("h:mm:ss A") },
      ],
      [
        { location: "London", t24h: moment().tz("Europe/London").format("H:mm:ss"), t12h: moment().tz("Europe/London").format("h:mm:ss A") },
        { location: "Berlin", t24h: moment().tz("Europe/Berlin").format("H:mm:ss"), t12h: moment().tz("Europe/Berlin").format("h:mm:ss A") },
        { location: "Kyiv", t24h: moment().tz("Europe/Kiev").format("H:mm:ss"), t12h: moment().tz("Europe/Kiev").format("h:mm:ss A") },
        { location: "Moscow", t24h: moment().tz("America/New_York").format("H:mm:ss"), t12h: moment().tz("America/New_York").format("h:mm:ss A") },
      ],
      [
        { location: "Shanghai", t24h: moment().tz("Asia/Shanghai").format("H:mm:ss"), t12h: moment().tz("Asia/Shanghai").format("h:mm:ss A") },
        { location: "Tokyo", t24h: moment().tz("Asia/Tokyo").format("H:mm:ss"), t12h: moment().tz("Asia/Tokyo").format("h:mm:ss A") },
        { location: "Sydney", t24h: moment().tz("Australia/Sydney").format("H:mm:ss"), t12h: moment().tz("Australia/Sydney").format("h:mm:ss A") },
        { location: "Auckland", t24h: moment().tz("Pacific/Auckland").format("H:mm:ss"), t12h: moment().tz("Pacific/Auckland").format("h:mm:ss A") },
      ],
    ],

    nextShow: "",
  }),
  mounted: function () {
    clearInterval(this.timeInterval);
    this.getShows();
    this.updateTime();
  },
  beforeDestroy() {
    clearInterval(this.timeInterval);
  },
  computed: {
    darkModeEnabled: function () {
      return this.$vuetify.theme.dark;
    },
  },
  methods: {
    toggleDarkMode: function () {
      if (this.alwaysDarkMode || moment().format("H") >= 20 || moment().format("H") < 6) {
        this.$vuetify.theme.dark = true;
      } else {
        this.$vuetify.theme.dark = false;
      }
    },
    updateTime: function () {
      const objThis = this;
      objThis.timeInterval = setInterval(function () {
        objThis.mainTime.time = moment().tz("UTC").format("H:mm:ss");

        objThis.rows = [
          [
            { location: "Los Angeles", t24h: moment().tz("America/Los_Angeles").format("H:mm:ss"), t12h: moment().tz("America/Los_Angeles").format("h:mm:ss A") },
            { location: "Denver", t24h: moment().tz("America/Denver").format("H:mm:ss"), t12h: moment().tz("America/Denver").format("h:mm:ss A") },
            { location: "Houston", t24h: moment().tz("US/Central").format("H:mm:ss"), t12h: moment().tz("US/Central").format("h:mm:ss A") },
            { location: "New York", t24h: moment().tz("America/New_York").format("H:mm:ss"), t12h: moment().tz("America/New_York").format("h:mm:ss A") },
          ],
          [
            { location: "Hawaii", t24h: moment().tz("US/Hawaii").format("H:mm:ss"), t12h: moment().tz("US/Hawaii").format("h:mm:ss A") },
            { location: "Alaska", t24h: moment().tz("US/Alaska").format("H:mm:ss"), t12h: moment().tz("US/Alaska").format("h:mm:ss A") },
            { location: "Phoenix", t24h: moment().tz("US/Arizona").format("H:mm:ss"), t12h: moment().tz("US/Arizona").format("h:mm:ss A") },
            { location: "Buenos Aires", t24h: moment().tz("America/Argentina/Buenos_Aires").format("H:mm:ss"), t12h: moment().tz("America/Argentina/Buenos_Aires").format("h:mm:ss A") },
          ],
          [
            { location: "London", t24h: moment().tz("Europe/London").format("H:mm:ss"), t12h: moment().tz("Europe/London").format("h:mm:ss A") },
            { location: "Berlin", t24h: moment().tz("Europe/Berlin").format("H:mm:ss"), t12h: moment().tz("Europe/Berlin").format("h:mm:ss A") },
            { location: "Kyiv", t24h: moment().tz("Europe/Kiev").format("H:mm:ss"), t12h: moment().tz("Europe/Kiev").format("h:mm:ss A") },
            { location: "Moscow", t24h: moment().tz("America/New_York").format("H:mm:ss"), t12h: moment().tz("America/New_York").format("h:mm:ss A") },
          ],
          [
            { location: "Shanghai", t24h: moment().tz("Asia/Shanghai").format("H:mm:ss"), t12h: moment().tz("Asia/Shanghai").format("h:mm:ss A") },
            { location: "Tokyo", t24h: moment().tz("Asia/Tokyo").format("H:mm:ss"), t12h: moment().tz("Asia/Tokyo").format("h:mm:ss A") },
            { location: "Sydney", t24h: moment().tz("Australia/Sydney").format("H:mm:ss"), t12h: moment().tz("Australia/Sydney").format("h:mm:ss A") },
            { location: "Auckland", t24h: moment().tz("Pacific/Auckland").format("H:mm:ss"), t12h: moment().tz("Pacific/Auckland").format("h:mm:ss A") },
          ],
        ];
        objThis.today = moment().tz("America/Denver").format("MMMM Do");
        objThis.todayYear = moment().tz("America/Denver").format("YYYY");
        objThis.tomorrow = moment().tz("America/Denver").add(1, "day").format("MMMM Do");
        objThis.tomorrowYear = moment().tz("America/Denver").add(1, "day").format("YYYY");

        objThis.nextShow = objThis.getNextShow();

        if (moment().minutes() == "00") {
          objThis.getShows();
        }
        
        objThis.displayedRow1 = objThis.rows[0];
        objThis.displayedRow2 = objThis.rows[objThis.rowIteration];

        if (moment().seconds() % 5 == 4) {
          objThis.rowIteration++;
        }

        if (objThis.rowIteration > 3) {
          objThis.rowIteration = 1;
        }

        if (objThis.alwaysDarkMode && objThis.darkModeEnabled) {
          return;
        } else {
          objThis.toggleDarkMode();
        }
      }, 1000);
    },
    getShows: async function () {
      const res = await fetch("https://api.unknower.io/event");
      const data = await res.json();
      this.showList = data.events;
    },
    getNextShow: function () {
      if (this.showList) {
        const nextShow = this.showList.find((show) => moment(show.startTime).isAfter());
        nextShow.fromNow = moment(nextShow.startTime).fromNow();
        nextShow.displayedTime = moment(nextShow.startTime).tz("America/Denver").format("hh:mm a");
        return nextShow;
      }
    },
  },
};
</script>

<style scoped>
h1 {
  font-size: 56pt;
}
h2 {
  font-size: 38pt;
}
.date h2 {
  font-size: 28pt;
  line-height: 32pt;
  margin-bottom: 12px;
}
.date h3 {
  font-size: 24pt;
  line-height: 26pt;
}
.next {
  text-transform: uppercase;
}
</style>
