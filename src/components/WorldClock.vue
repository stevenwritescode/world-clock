<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="3" class="date">
        <v-card class="pa-4">
          <h2>TODAY</h2>
          <h3>{{ today }}</h3>
          <h2>{{ todayYear }}</h2>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col cols="4">
        <v-card class="pa-4">
          <h2>UTC</h2>
          <h1>{{ utc }}</h1>
        </v-card>
      </v-col>
      <v-spacer></v-spacer>
      <v-col cols="3" class="date">
        <v-card class="pa-4">
          <h2>TOMORROW</h2>
          <h3>{{ tomorrow }}</h3>
          <h2>{{ tomorrowYear }}</h2>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>DENVER</h2>
          <h1>{{ denver }}</h1>
          <h3>{{ denver12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>NEW YORK</h2>
          <h1>{{ newyork }}</h1>
          <h3>{{ newyork12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>LONDON</h2>
          <h1>{{ london }}</h1>
          <h3>{{ london12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>BERLIN</h2>
          <h1>{{ berlin }}</h1>
          <h3>{{ berlin12h }}</h3>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>MOSCOW</h2>
          <h1>{{ moscow }}</h1>
          <h3>{{ moscow12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>TOKYO</h2>
          <h1>{{ tokyo }}</h1>
          <h3>{{ tokyo12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>SYDNEY</h2>
          <h1>{{ sydney }}</h1>
          <h3>{{ sydney12h }}</h3>
        </v-card>
      </v-col>
      <v-col cols="3">
        <v-card class="pa-4">
          <h2>AUCKLAND</h2>
          <h1>{{ auckland }}</h1>
          <h3>{{ auckland12h }}</h3>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col cols="12">
        <h2 style="text-transform: uppercase">NEXT SHOW IS {{ nextShow }}</h2>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import moment from "moment-timezone";
export default {
  name: "WorldClock",

  data: () => ({
    timeInterval: "",
    today: moment().tz("America/Denver").format("MMMM Do"),
    todayYear: moment().tz("America/Denver").format("YYYY"),
    tomorrow: moment().tz("America/Denver").add(1, "day").format("MMMM Do"),
    tomorrowYear: moment().tz("America/Denver").add(1, "day").format("YYYY"),
    utc: moment().tz("UTC").format("H:mm:ss"),
    newyork: moment().tz("America/New_York").format("H:mm:ss"),
    newyork12h: moment().tz("America/New_York").format("h:mm:ss A"),
    denver: moment().tz("America/Denver").format("H:mm:ss"),
    denver12h: moment().tz("America/Denver").format("h:mm:ss A"),
    london: moment().tz("Europe/London").format("H:mm:ss"),
    london12h: moment().tz("Europe/London").format("h:mm:ss A"),
    berlin: moment().tz("Europe/Berlin").format("H:mm:s"),
    berlin12h: moment().tz("Europe/Berlin").format("h:mm:ss A"),
    moscow: moment().tz("Europe/Moscow").format("H:mm:s"),
    moscow12h: moment().tz("Europe/Moscow").format("h:mm:ss A"),
    tokyo: moment().tz("Asia/Tokyo").format("H:mm:s"),
    tokyo12h: moment().tz("Asia/Tokyo").format("h:mm:ss A"),
    sydney: moment().tz("Australia/Sydney").format("H:mm:s"),
    sydney12h: moment().tz("Australia/Sydney").format("h:mm:ss A"),
    auckland: moment().tz("Pacific/Auckland").format("H:mm:s"),
    auckland12h: moment().tz("Pacific/Auckland").format("h:mm:ss A"),
    nextShow: "",
  }),
  mounted: function () {
    clearInterval(this.timeInterval);
    this.updateTime();
  },
  beforeDestroy() {
    clearInterval(this.timeInterval);
  },
  methods: {
    toggleDarkMode: function () {
      if (moment().format("H") > 20 || moment().format("H") < 6) {
        this.$vuetify.theme.dark = true;
      } else {
        this.$vuetify.theme.dark = false;
      }
    },
    updateTime: function () {
      const data = this;
      setInterval(function () {
        data.utc = moment().tz("UTC").format("H:mm:ss");
        data.newyork = moment().tz("America/New_York").format("H:mm:ss");
        data.newyork12h = moment().tz("America/New_York").format("h:mm:ss A");
        data.denver = moment().tz("America/Denver").format("H:mm:ss");
        data.denver12h = moment().tz("America/Denver").format("h:mm:ss A");
        data.london = moment().tz("Europe/London").format("H:mm:ss");
        data.london12h = moment().tz("Europe/London").format("h:mm:ss A");
        data.berlin = moment().tz("Europe/Berlin").format("H:mm:ss");
        data.berlin12h = moment().tz("Europe/Berlin").format("h:mm:ss A");
        data.moscow = moment().tz("Europe/Moscow").format("H:mm:ss");
        data.moscow12h = moment().tz("Europe/Moscow").format("h:mm:ss A");
        data.tokyo = moment().tz("Asia/Tokyo").format("H:mm:ss");
        data.tokyo12h = moment().tz("Asia/Tokyo").format("h:mm:ss A");
        data.sydney = moment().tz("Australia/Sydney").format("H:mm:ss");
        data.sydney12h = moment().tz("Australia/Sydney").format("h:mm:ss A");
        data.auckland = moment().tz("Pacific/Auckland").format("H:mm:ss");
        data.auckland12h = moment().tz("Pacific/Auckland").format("h:mm:ss A");
        data.today = moment().tz("America/Denver").format("MMMM Do");
        data.todayYear = moment().tz("America/Denver").format("YYYY");
        data.tomorrow = moment()
          .tz("America/Denver")
          .add(1, "day")
          .format("MMMM Do");
        data.tomorrowYear = moment()
          .tz("America/Denver")
          .add(1, "day")
          .format("YYYY");
        data.nextShow = data.getNextShow();

        data.toggleDarkMode();
      }, 500);
    },
    getNextShow: function () {
      const firstShow = moment("2022-02-22T08:00:00Z");
      const nextSaturday = moment.utc().isoWeekday(6).format("YYYY-MM-DD");
      const nextSunday = moment.utc().isoWeekday(7).format("YYYY-MM-DD");
      const now = moment.utc();

      let australiaShow = `${nextSaturday}T08:00:00Z`;
      const europeShow = `${nextSaturday}T19:00:00Z`;
      const americasShow = `${nextSunday}T01:00:00Z`;

      if (
        moment(australiaShow).diff(now, "days") < 7 &&
        moment(australiaShow).diff(now, "days") > 0
      ) {
        australiaShow = firstShow;
      }

      if (now.isBefore(australiaShow)) {
        return `for Asia/Pacific ${moment(australiaShow).fromNow()} at ${moment(
          australiaShow
        ).format("h:mm:ss A")}`;
      } else if (now.isBefore(europeShow)) {
        return `for Europe ${moment(europeShow).fromNow()} at ${moment(
          europeShow
        ).format("h:mm:ss A")}`;
      } else if (now.isBefore(americasShow)) {
        return `for The Americas ${moment(americasShow).fromNow()} at ${moment(
          americasShow
        ).format("h:mm:ss A")}`;
      } else {
        return "Unknown";
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
</style>
