<template>
  <div class="text-center">
    <h2 class="text-3xl font-bold">
        {{ text }}
    </h2>
    <div class="text-2xl mt-4 mb-10">
        {{ timestamp }}
    </div>
  </div>
</template>

<script>
import { computed } from 'vue';
import moment from 'moment';

export default {
    name: 'DataTitle',
    props: {
        text: String,
        dataDate: String,
    },
    setup(props) {
        const timestamp = computed(() => {return moment(props?.dataDate).format('MMMM Do YYYY hh:mm:ss a')});
       
        return { timestamp }
    },
}
</script>