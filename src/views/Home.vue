<template>
  <main v-if="!loading" class="home">
    <DataTitle :text='title' :dataDate="dataDate" />
    <DataBoxes :stats='stats' />
    <CountrySelect @get-country='getCountryData' :countries='countries' />
    <div class="flex justify-center grid grid-rows-3 grid-flow-col gap-4">
      <div class="row-start-1 row-end-4  flex items-center">
        <button @click.prevent="goBackChart()" class="bg-green-500 rounded p-5 mt-5 mx-5 pb-4 text-white">
        Go Back
        </button>
      </div>
      <div class="row-start-1 row-end-4">
        <Chart :countries='countries' :min='min' :max='max' />
      </div>
      <div class="row-start-1 row-end-4 flex items-center">
        <button @click.prevent="seeNextChart()" class="bg-green-500 rounded p-5 mt-5 mx-5 pb-4 text-white">
        See next
        </button>
      </div>
    </div>
    <div class="grid grid-rows-3 grid-flow-col gap-4 mt-5">
      <div class="flex justify-center mb-10">
        <button class="bg-green-500 rounded p-5 mt-5 pb-4 text-white" 
          @click.prevent='clearCountryData()'>
          Clear data
      </button>
        
      </div>
      <div class="flex justify-center mt-10">
      </div>
    </div>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
  <div class="text-gray-500 text-3xl mt-10 mb-6">
    Fetching data
  </div>
  <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
  <div class="pb-10"></div>
</template>

<script>
import DataTitle from '@/components/Data.Title.vue';
import DataBoxes from '@/components/Data.Boxes.vue';
import CountrySelect from '@/components/Country.Select.vue';
import Chart from '@/components/Chart.vue';

export default {
  name: 'Home',
   data () {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      status: {},
      countries: [],
      loadingImage: require('../assets/loader.gif'),
      min: 0,
      max: 3,
    }
  },
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    Chart
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global;
      this.loading = false;
      this.min = 0;
      this.max = 3;
    },
     seeNextChart() {
      this.min++;
      this.max++;
    },
    goBackChart() {
      this.min--;
      this.max--;
    }
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading =  false;
  },
}
</script>
