<script setup>
import { RouterLink } from 'vue-router';
defineProps({
    country: Object
})


</script>

<template>
    
    <RouterLink class="country" :to="'/country/' + country.name.common">
        <img :src="country.flags.png" alt="">
        <h3>{{ country.name.common }}</h3>
        <p><span class="bold">Population: </span>{{ country.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ") }}</p>
        <p><span class="bold">Region: </span>{{ country.region }}</p>
        <p><span class="bold">Capital: </span>{{ country.capital ? country.capital[0] : "" }}</p>
    </RouterLink>
    
</template>

<style scoped>
    .country{
        width: 20%;
        font-size: 1.5rem;
        border-radius: 15px;    
        box-shadow: 0px 0px 15px 0px rgba(0,0,0,0.75);  
        padding-bottom: 4%;
        margin: 5% 5% 0 0;
        cursor: pointer;
        text-decoration: none;
        color: #000;
    }

    .bold{
        font-weight: bold;
    }

    p, h3{
        padding: 0 5%;
    }

    h3{
        margin: 5% 0;
    }

    img{
        width: 100%;
        height: 200px;
        border-top-right-radius: 15px;
        border-top-left-radius: 15px;
    }
</style>