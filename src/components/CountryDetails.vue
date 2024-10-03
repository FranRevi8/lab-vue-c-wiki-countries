<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";
import { watch } from "vue";

const route = useRoute();
const routeParam = route.params.alpha3Code;

const countries = ref([]);
const country = ref();
const imgUrl = ref();

const fetchCountries = async () => {
  try {
    const response = await fetch("/countries.json");
    const data = await response.json();
    countries.value = data;
    country.value = countries.value.find(
      (country) => (country.alpha3Code === routeParam)
    );
    imgUrl.value = ("https://flagpedia.net/data/flags/icon/72x54/" + country.value.alpha2Code.toLowerCase() + ".png")
    
  } catch (error) {
    console.error("Error cargando el archivo JSON:", error);
  }
};

fetchCountries();

watch(() => route.params.alpha3Code, (newAlpha3Code) => {
  country.value = countries.value.find(
    (c) => c.alpha3Code === newAlpha3Code
  );
  if (country.value) {
    imgUrl.value = `https://flagpedia.net/data/flags/icon/72x54/${country.value.alpha2Code.toLowerCase()}.png`;
  } else {
    imgUrl.value = "";
  }
});

</script>

<template>
  <div class="container-fluid">
    <div class="row">
      <div class="main-content col-md-5 bg-secondary p-2 position-absolute">
        <div v-if="country && imgUrl">
            <img :src = imgUrl>
            <h2>{{country.name.common}}</h2>
            <p>Capital</p>
            <p>Area</p>
            <p>Borders</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.position-absolute {
  top: 200px;
  left: 45%;
}
</style>
