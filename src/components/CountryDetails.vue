<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";
import { watch } from "vue";

const route = useRoute();
const routeParam = route.params.alpha3Code;

const countries = ref();
const country = ref();
const imgUrl = ref();
const area = ref();
const borders = ref();
const capital = ref();

const a3cToName = (a3c) => {
  const country = countries.value.find((c) => c.alpha3Code === a3c);
  return country.name.common;
};

const fetchCountry = async (alpha3Code) => {
  try {
    const response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries/" + alpha3Code
    );
    const data = await response.json();
    country.value = data;

    const responsePais = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    const dataPais = await responsePais.json();
    countries.value = dataPais;

    imgUrl.value =
      "https://flagpedia.net/data/flags/icon/72x54/" +
      country.value.alpha2Code.toLowerCase() +
      ".png";
    area.value = country.value.area;
    capital.value = country.value.capital;
    borders.value = country.value.borders;
  } catch (error) {
    console.error("Error cargando el archivo JSON:", error);
  }
};

fetchCountry(routeParam);

watch(
  () => route.params.alpha3Code,
  (newAlpha3Code) => {
    fetchCountry(newAlpha3Code);
  }
);
</script>

<template>
  <div class="container-fluid" v-if="country">
    <div class="row">
      <div
        class="main-content col-md-5 p-2 position-fixed"
        style="height: 750px"
      >
        <div v-if="country && imgUrl" class="text-center">
          <img :src="imgUrl" class="mt-5" />
          <h2 class="mt-4">{{ country.name.common }}</h2>
          <div class="block">
            <p>Capital</p>
            <p>{{ capital[0] }}</p>
          </div>
          <div class="separator"></div>
          <br />
          <div class="block">
          <p>Area</p>
          <p>{{ area }}</p>
          </div>
          <div class="separator"></div>
          <br />
          <div v-if="country.borders.length > 0" class="block2">
            <p>Borders</p>
            <ul>
              <li v-for="border in country.borders">
                <router-link :to="`/list/${border}`">{{
                  a3cToName(border)
                }}</router-link>
              </li>
            </ul>
            <div class="separator"></div>
          </div>
          <div v-else>
            <p>Borders</p>
            <p>Does not have any</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.position-fixed {
  top: 120px;
  right: 8.33%;
}

img {
  height: 150px;
}

.separator {
  height: 2px;
  background-color: lightgrey;
  border-radius: 1%;
}

.block {
  display: flex;
  justify-content: space-around;
  align-content: center;
  align-items: center;
  margin-top: 50px;
  font-weight: 700;
}

.block2 {
  display: flex;
  justify-content: space-around;
  align-content: center;
  align-items: center;
  margin-top: 50px;
  font-weight: 700;
  width: 140%;
}

li{
    list-style-type: none;
}
</style>
