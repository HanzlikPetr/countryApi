<script setup>
import { onMounted, reactive, ref } from 'vue';
import axios from 'axios';
import CountryComponent from '@/components/CountryComponent.vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

const searchCountry = ref('')
const region = ref('Filter by region')
const state = reactive({
  countries: Object,
  isLoading: true
})
const limit = ref(1)

const moreCountries = () => {
  limit.value++;
}

const lessCountries = () => {
  limit.value--;
}

const search = async () => {
  state.isLoading = true
  limit.value = 1
  try {
      const response  = await axios.get("https://restcountries.com/v3.1/name/" + searchCountry.value)
      state.countries = response.data
  } catch (error) {
    console.log(error)
  }finally{
    state.isLoading = false
  }
}

const filter = async () => {
  state.isLoading = true
  limit.value = 1
  try {
      const response  = await axios.get("https://restcountries.com/v3.1/region/" + region.value)
      state.countries = response.data
  } catch (error) {
    console.log(error)
  }finally{
    state.isLoading = false
  }
}

onMounted(async () => {
  try {
      const response  = await axios.get("https://restcountries.com/v3.1/all")
      state.countries = response.data
  } catch (error) {
    console.log(error)
  }finally{
    state.isLoading = false
  }
})
</script>

<template>
  <main>
    <div class="searchFilter">
      <input type="text" v-model="searchCountry" @keydown="search" placeholder="Find coutry">
      <select v-model="region" @change.prevent="filter">
        <option disabled>Filter by region</option>
        <option >Africa</option>
        <option >America</option>
        <option >Asia</option>
        <option >Europe</option>
        <option >Oceania</option>
      </select>
    </div>
    <div v-if="!state.isLoading">
      <div class="countries">
        <CountryComponent v-for="country in state.countries.slice(0,8 * limit)" :key="country.name.common" :country = "country"/>
      </div>
      <div class="buttons" :class="(limit === 1 || (limit + 1) * 8 > state.countries.length) ? 'alone' : ''">
        <button v-if="limit > 1" @click="lessCountries">Less countries</button>
        <button v-if="(limit + 1) * 8 < state.countries.length" @click="moreCountries">More countries</button>
      </div>
    </div>
    <PulseLoader v-else/>
    
  </main>
</template>

<style scoped>
  .searchFilter{
    width: 70%;
    margin-left: -5%;
    display: flex;
    justify-content: space-between;
    margin-top: 2%;
  }

  .searchFilter input{
    font-size: 150%;
    border-radius: 15px;
    border: solid 5px;
    padding: 1% 3%;
  }

  .searchFilter select{
    font-size: 150%;
    border-radius: 15px;
    border: solid 5px;
    padding-right: 5%;
  }

  .countries{
    display: flex;
    flex-wrap: wrap;
    width: 80vw;
    justify-content: space-between;
    margin: auto;
  }

  button{
    width: 20%;
    margin: 2% 0;
    border: solid 4px #000;
    font-size: 150%;
    padding: 1%;
    font-weight: 700;
    border-radius: 15px;
    cursor: pointer;
  }

  .buttons{
    width: 60%;
    display: flex;
    justify-content: space-between;
    margin-left: -5%;
  }

  .alone{
    justify-content: center;
  }
</style>