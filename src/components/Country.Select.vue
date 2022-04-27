<template>
<div class="flex flex-col items-center">
  <h3 class="font-bold text-2xl mt-7">Country selector:</h3>
  <div class="w-3/5">
    <select 
        @change="onChange()" 
        v-model='selected' 
        class="form-select mt-10 ml-2 mr-2 block w-full border p-3 rounded">
        <option value="0">Select a country</option>
        <option 
          v-for="country in countries" 
          :key='country.ID' 
          :value='country.ID'> 
          {{country.Country}}
        </option>
    </select>
  </div>
</div>

</template>

<script>
export default {
    name: 'CountrySelect',
    props: ['countries'],
    data() {
      return {
        selected: 0,
      }
    },
    methods: {
      onChange() {
        const country = this.countries.find((item) => item.ID === this.selected);

        this.$emit('get-country', country);
      },
    },
}
</script>
