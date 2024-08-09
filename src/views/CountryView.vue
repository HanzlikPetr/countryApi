<script setup>
import { onMounted, reactive, ref } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import { RouterLink } from 'vue-router'

const route = useRoute()
const countryName = route.params.name
const state = reactive({
    country: Object,
    isLoading: true
})

const borders = ref([])


onMounted(async () => {
    try {
      const response  = await axios.get("https://restcountries.com/v3.1/name/" + countryName)
      state.country = response.data[0]

      state.country.borders.forEach(async element => {
        const responseBorders = await axios.get("https://restcountries.com/v3.1/alpha/" + element)
        borders.value.push(responseBorders.data[0].name.common)
      });

  } catch (error) {
    console.log(error)
  }finally{
    state.isLoading = false
  }
})
</script>

<template>
    <RouterLink to="/" class="linkHome">Back</RouterLink>
    <div v-if="!state.isLoading">
        <img :src="state.country.flags.png" alt="">
        <div>
            <h3>{{ state.country.name.common }}</h3>
            <div>
                <p><span class="bold">Native Name: </span>{{ state.country.name.nativeName[Object.keys(state.country.name.nativeName)[0]].official }}</p>
                <p><span class="bold">Population: </span>{{ state.country.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")  }}</p>
                <p><span class="bold">Region: </span>{{ state.country.region }}</p>
                <p><span class="bold">Sub Region: </span>{{ state.country.subregion }}</p>
                <p><span class="bold">Capital: </span>{{ state.country.capital[0] }}</p>
            </div>
            <div>
                <p><span class="bold">Top Level Domain: </span>{{ state.country.tld[0] }}</p>
                <p><span class="bold">Currencies: </span>{{ state.country.currencies[Object.keys(state.country.currencies)[0]].name }}</p>
                <p><span class="bold">Languages: </span>{{ state.country.languages[Object.keys(state.country.languages)[0]] }}</p>
            </div>
            <div>
                <h4>Borders:</h4>
                <p v-for="border in borders" :key="border">{{ border }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .linkHome{
            display: block;
            text-decoration: none;
            color: #000;
            width: 80%;
            margin: auto;
            font-size: 150%;
            padding: 1% 0;
            
        }
</style>