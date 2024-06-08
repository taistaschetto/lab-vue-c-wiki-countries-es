<template>
  <div class="country-details" v-if="country.name">
    <h2 v-if="country.name">{{ country.name.common }}</h2>
    <p v-if="country.capital">{{ country.capital[0] }}</p>
    <img
      :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
      alt="Country Flag"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";

const country = ref([]);
const route = useRoute();

const fetchCountryDetails = async (alpha3Code) => {
  const response = await fetch(
    'https://ih-countries-api.herokuapp.com/countries/' + alpha3Code
  );
  const data = await response.json();
  console.log(data);
  country.value = data
};

onMounted(() => {
  if (route.params.alpha3Code) {
    console.log(route.params.alpha3Code)
    fetchCountryDetails(route.params.alpha3Code);
  }
});

watch(
  () => route.params.alpha3Code ,
  (newVal) => {
    fetchCountryDetails(newVal);
  },
  { immediate: true }
);
</script>
