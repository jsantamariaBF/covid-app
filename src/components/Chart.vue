<template>
  <div class="flex justify-center mt-10">
    <Chart
      :size="{ width:420, height: 420 }"
      :data="countries.slice(min, max)"
      :margin="margin"
      :direction="direction"
      :axis="axis">
      <template #layers>
        <Grid strokeDasharray="2,2" />
        <Line :dataKeys="['Country', 'TotalConfirmed']" :lineStyle="{ stroke: 'red' }" type="monotone" />
        <Line :dataKeys="['Country', 'TotalDeaths']" :lineStyle="{ stroke: 'green' }" type="monotone" />

        <!-- <Line :dataKeys="['name', 'avg']" :lineStyle="{ stroke: 'red' }" type="step" /> -->
      </template>
    </Chart>
  </div>
  
</template>

<script>
import { defineComponent, ref, reactive } from 'vue'
import { Chart, Grid, Line } from 'vue3-charts'

export default {
  name: 'LineChart',
  components: { Chart, Grid, Line },
  props: {
    min: Number,
    max: Number,
    countries: Array,
  },   
  setup(props) {
    const direction = ref('horizontal');
    const countryName = ref([]);
    const numOfInfected = ref([]);
    const margin = reactive({
      left: 0,
      top: 10,
      right: 20,
      bottom: 0
    });

    const axis = ref({
      primary: {
        // type: 'band',
        // format: (val) => {
        //   if (val === 'Feb') {
        //     return '😜'
        //   }
        //   return val
        // }
      },
      secondary: {
        domain: ['dataMin', 'dataMax + 100'],
        type: 'linear',
        ticks: 8
      }
    });
    
    function getCountryAndInfected() {
      countryName.value = [];
      numOfInfected.value = [];
      props.countries.map((countriesName) => countryName.value.push(countriesName.Country));
      props.countries.map((numInfected) => numOfInfected.value.push(numInfected.TotalConfirmed));
    };

      getCountryAndInfected();

    return { direction, margin, axis }
  }
}
</script>