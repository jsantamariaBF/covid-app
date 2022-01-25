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
        <Line :dataKeys="['CountryCode', 'TotalConfirmed']" :lineStyle="{ stroke: 'red' }" type="monotone" />
        <!-- <Line :dataKeys="['name', 'avg']" :lineStyle="{ stroke: 'red' }" type="step" /> -->
      </template>
    </Chart>
  </div>
  
</template>

<script>
import { defineComponent, ref } from 'vue'
import { Chart, Grid, Line } from 'vue3-charts'

export default defineComponent({
  name: 'LineChart',
    data () {
    return {
    countryName: [],
    numOfInfected: [],
    }
  },
  components: { Chart, Grid, Line },
  props: ['countries', 'min', 'max'],   
  methods: {
    getCountryAndInfected() {
    this.countryName = [];
    this.numOfInfected = [];

    this.countries.map((countriesName) => this.countryName.push(countriesName.Country));
    this.countries.map((numInfected) => this.numOfInfected.push(numInfected.TotalConfirmed));
    },
  },
  computed: {
     
  },
  created(){
    this.getCountryAndInfected();
  },
  setup() {
    const direction = ref('horizontal')
    const margin = ref({
      left: 0,
      top: 10,
      right: 20,
      bottom: 0
    })

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
    })
    return { direction, margin, axis }
  }
})
</script>