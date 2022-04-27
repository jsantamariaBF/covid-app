<template>
  <main v-if="!loading.chart" class="home">
    <DataTitle :text='title' :dataDate="dataDate" />
    <DataBoxes :stats='stats' />
    <CountrySelect @get-country='getCountryData' :countries='countries' />
    <div class="flex justify-center">
      <Chart :countries='countries' :min='min' :max='max' />
      <div class="flex items-center ml-5">
        <ul class="list-disc">
          <li class="text-red-700 font-bold">Total Infected</li>
          <li class="text-green-700 font-bold">Total Deaths</li>
        </ul>
      </div>
    </div>
    <div class="flex justify-center mt-10">
      <button class="bg-green-500 rounded p-5 mx-5 pb-4 text-white" 
        @click='clearCountryData'>
        Clear data
      </button>
      <button @click="goBackChart" class="bg-green-500 rounded p-5 mx-5 pb-4 text-white">
        Go Back
      </button>
      <button @click="seeNextChart" class="bg-green-500 rounded p-5 mx-5 pb-4 text-white">
        See next
      </button>
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
import {ref, computed, reactive} from 'vue';

import DataTitle from '@/components/Data.Title.vue';
import DataBoxes from '@/components/Data.Boxes.vue';
import CountrySelect from '@/components/Country.Select.vue';
import Chart from '@/components/Chart.vue';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    Chart
  },
  setup() {
    const loading = reactive({
      chart: false,
    });
    const title = ref('Global');
    const dataDate = ref('');
    const status = reactive({});
    const countries = ref([]);
    const min = ref(0);
    const max = ref(5);
    const stats = ref();
    const loadingImage = require('../assets/loader.gif');

    async function fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();
      return data;
    };
   
    function getCountryData(country) {
      stats.value = country;
      title.value = country.Country;
    };

     async function clearCountryData() {
      loading.chart = true;
      const data = await fetchCovidData();
      title.value = 'Global';
      stats.value = data.Global;
      loading.chart = false;
      min.value = 0;
      max.value = 3;
    };

    function seeNextChart() {
      min.value++;
      max.value++;
    };

    function goBackChart() {
      min.value--;
      max.value--;
    };

    async function created() {
      const data = await fetchCovidData();
      dataDate.value = data.Date;
      stats.value = data.Global;
      countries.value = data.Countries;
      loading.chart = false;
    };
    created();

    return {
      loading,
      title,
      dataDate,
      status,
      loadingImage,
      countries,
      min,
      max,
      fetchCovidData,
      getCountryData,
      clearCountryData,
      seeNextChart,
      goBackChart,
      created,
      stats,
    }
  }
}
</script>

<style >
 
</style>

