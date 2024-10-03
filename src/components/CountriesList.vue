<script setup>
import { ref } from 'vue';

const countries = ref([]);

const fetchCountries = async () => {
  try {
    const response = await fetch('/countries.json');
    const data = await response.json();
    countries.value = data;
  } catch (error) {
    console.error('Error cargando el archivo JSON:', error);
  }
};

fetchCountries();
</script>

<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-1 bg-white"></div>
      <div class="main-content col-md-10 bg-white p-2">
      <ul>
        <li v-for="country in countries" :key="country.alpha3Code">
          <router-link :to="`/list/${country.alpha3Code}`">{{ country.name.common }}</router-link>
        </li>
      </ul>
      <router-view></router-view>
      </div>
      <div class="col-md-1 bg-white"></div>
    </div>
  </div>
</template>

<style scoped>
</style>