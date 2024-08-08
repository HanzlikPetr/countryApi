<script setup>
import { ref } from 'vue';


const searchCountry = ref('')
const region = ref('Filter by region')
</script>

<template>
  <main>
    <div>
      <input type="text" v-model="searchCountry">
      <select v-model="region">
        <option disabled>Filter by region</option>
        <option >Africa</option>
        <option >America</option>
        <option >Asia</option>
        <option >Europe</option>
        <option >Oceania</option>
      </select>
    </div>
  </main>
</template>
