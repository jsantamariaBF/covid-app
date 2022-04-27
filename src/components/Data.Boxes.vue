<template>
    <div class="grid md:grid-cols-2 gap-4">
        <div class="shadow-md bg-blue-100 p-10 text-center rounded">
            <h3 class="text-blue-900 font-bold text-3xl mb-4">Cases</h3>
            <div class="text-2xl mb-4">
                <span class="font-bold">New: {{ NewConfirmed }}</span>
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total: {{ TotalConfirmed }}</span>
            </div>
        </div>
        <div class="shadow-md bg-blue-100 p-10 text-center rounded">
            <h3 class="text-blue-900 font-bold text-3xl mb-4">Deaths</h3>
            <div class="text-2xl mb-4">
                <span class="font-bold">New: {{ NewDeaths }}</span>
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">Total: {{ TotalDeaths }}</span>
            </div>
        </div>
    </div>
</template>

<script>
import { computed } from 'vue';

export default {
    name: 'DataBoxes',
    props: {
        stats: Object,
    },
    setup(props) {
        const NewConfirmed = computed(() => { return numberWithCommas(props.stats?.NewConfirmed) });
        const TotalConfirmed = computed(() => { return numberWithCommas(props.stats?.TotalConfirmed) });
        const NewDeaths = computed(() => { return numberWithCommas(props.stats?.NewDeaths) });
        const TotalDeaths = computed(() => { return numberWithCommas(props.stats?.TotalDeaths) });
        
        function numberWithCommas(x) {
            return x?.toString()?.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
        };

    return {
        NewConfirmed, 
        TotalConfirmed, 
        NewDeaths, 
        TotalDeaths,
        numberWithCommas,
    }
    },
}
</script>


<style>

</style>