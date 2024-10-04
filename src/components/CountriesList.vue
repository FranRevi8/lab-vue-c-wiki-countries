<script setup>
import { ref } from "vue";

const countries = ref([]);

const fetchCountries = async () => {
  try {
    const response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    const data = await response.json();
    countries.value = data.sort((a, b) =>
      a.name.common.localeCompare(b.name.common)
    );
  } catch (error) {
    console.error("Error cargando el archivo JSON:", error);
  }
};

fetchCountries();
</script>

<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-1 bg-white"></div>
      <div class="main-content col-md-4 bg-white overflow-auto text-center">
        <ul>
          <li
            v-for="country in countries"
            :key="country.alpha3Code"
          >
            <router-link class="router-link" :to="`/list/${country.alpha3Code}`">
              <div class="border">
                <img
                  :src="
                    'https://flagpedia.net/data/flags/icon/72x54/' +
                    country.alpha2Code.toLowerCase() +
                    '.png'
                  "
                  alt="flag"
                  class="pt-3"
                />
                <br />
                <p class="text-secondary font-weight-bold mt-1">{{ country.name.common }}</p>
              </div>
            </router-link>
          </li>
        </ul>
      </div>
      <router-view></router-view>
      <div class="col-md-1 bg-white"></div>
    </div>
  </div>
</template>

<style scoped>
li {
  list-style-type: none;
}

.main-content {
  margin-top: 100px;
}

.overflow-auto {
  max-height: 750px;
}

.router-link{
  text-decoration: none;
}

.font-weight-bold{
  font-weight: 700;
}
</style>
