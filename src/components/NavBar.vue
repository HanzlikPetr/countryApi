<script setup>
import { RouterLink } from 'vue-router'

</script>

<template>
    <div class="wrapper">
      <nav>
        <RouterLink to="/" class="linkHome"><h1>Where is the wordl?</h1></RouterLink>
      </nav>
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

    .wrapper{
        box-shadow: 0px 0px 15px 0px rgba(0,0,0,0.75);
    }
</style>