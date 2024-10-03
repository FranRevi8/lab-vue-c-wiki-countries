<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";
import { watch } from "vue";

const route = useRoute();
const routeParam = route.params.alpha3Code;

const countries = ref([]);
const country = ref();
const imgUrl = ref();
const area = ref();
const borders = ref();
const capital = ref();

const a3cToName = (a3c) => {
  const country = countries.value.find(
    (c) => c.alpha3Code === a3c
  );
  
  return country.name.common;
};

const fetchCountries = async () => {
  try {
    const response = await fetch("/countries.json");
    const data = await response.json();
    countries.value = data;
    country.value = countries.value.find(
      (country) => (country.alpha3Code === routeParam)
    );

    imgUrl.value = ("https://flagpedia.net/data/flags/icon/72x54/" + country.value.alpha2Code.toLowerCase() + ".png")
    area.value = country.value.area
    capital.value = country.value.capital
    borders.value = country.value.borders
    
    
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
            <p>Capital</p><p>{{ capital[0] }}</p><br>
            <p>Area</p><p>{{ area }}</p><br>
            <p>Borders</p><ul>
                <li v-for="a3c in borders" :key="country.alpha3Code">
                    {{ a3cToName(a3c) }}
                </li>
            </ul>
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
