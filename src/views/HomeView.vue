<template>
  <div>
    <main v-if="!loading">
      <CountrySelect :countries="countries" @get-country="getCountryData" />

      <DataTitle :text="title" :dataDate="dataDate" />
      <DataBoxes :stats="stats" />
      <button
        @click="clearCountryData()"
        v-if="stats.Country"
        class="
          bg-blue-700
          text-white
          rounded
          p-3
          mt-10
          focus:outline-none
          hover:bg-blue-600
        "
      >
        Clear Country
      </button>
      <BarChart
        :allData="arrTotalConfirmed"
        :allDates="arrDates"
        label="Total Confirmed"
        backgroundColor="#be185d"
      />
      <BarChart
        :allData="arrTotalDeath"
        :allDates="arrDates"
        label="Total Deaths"
        backgroundColor="#b91c1c"
      />
      <BarChart
        :allData="arrIncidentRate"
        :allDates="arrDates"
        label="Incident Rates"
        backgroundColor="#0c4a6e"
      />
      <!-- <h1>Total deaths</h1> -->
      <!-- <BarChart :allData="arrTotalDeath" :allDates="arrDates" /> -->
    </main>
    <main v-else class="flex flex-col align-center justify-center text-center">
      <div class="text-gray-500 text-3xl mt-10 mb-6">fetching data</div>
      <img :src="loadingImage" class="w-24 m-auto" />
    </main>
  </div>
</template>

<script>
import DataTitle from "@/components/DataTitle.vue";
import DataBoxes from "@/components/DataBoxes.vue";
import CountrySelect from "@/components/CountrySelect.vue";
import moment from "moment";
import BarChart from "@/components/BarChart.vue";

export default {
  name: "HomeView",
  components: { DataTitle, DataBoxes, CountrySelect, BarChart },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/blue-hourglass.gif"),
      arrTotalDeath: [],
      arrTotalConfirmed: [],
      arrIncidentRate: [],
      //  arrTotalRecovered: [],
      arrDates: [],
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      console.log(data);
      return data;
    },
    async fetchDailyData() {
      const res = await fetch("https://covid19.mathdro.id/api/daily");
      const data = await res.json();
      console.log(data);
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = "Global";
      this.stats = data.Global;
      this.loading = false;
    },
  },
  async created() {
    this.loading = true;

    const data = await this.fetchCovidData();
    console.log(data);
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;

    //data values for chart visusalization.
    const dailyData = await this.fetchDailyData();
    const obj = dailyData;
    //console.log(obj);
    const arrA = [];
    const arrB = [];
    const arrC = [];
    const arrD = [];
    obj.forEach((element) => {
      const date = moment(element.reportDate).format("MM/DD");
      const TotalConfirmed = element.confirmed.china;
      const TotalDeaths = element.deaths.china;
      const incidentRate = element.incidentRate;
      // const TotalRecovered = element.recovered.china;
      //this.arrTotalConfirmed.push(TotalConfirmed);
      arrA.push(date);
      arrB.push(TotalConfirmed);
      arrC.push(TotalDeaths);
      arrD.push(incidentRate);
      //this.arrDates.push(date);
      //this.arrTotalDeath.push(TotalDeaths);
      //this.arrTotalRecovered.push(TotalRecovered);
    });

    this.arrDates = arrA;
    this.arrTotalConfirmed = arrB;
    this.arrTotalDeath = arrC;
    this.arrIncidentRate = arrD;
    this.loading = false;
  },
};
</script>
