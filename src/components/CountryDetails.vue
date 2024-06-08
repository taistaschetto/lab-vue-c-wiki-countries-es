<template>
  <div class="country-details">
    <h2>{{ country.name.common }}</h2>
    <p>Capital: {{ country.capital[0] }}</p>
    <img
      :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code?.toLowerCase()}.png`"
      alt="Country Flag"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';

const country = ref({});

const route = useRoute();

async function fetchCountryDetails(alpha3Code) {
  const url = `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error('Failed to fetch country details');
    }
    const data = await response.json();
    country.value = data[0]; 
    console.log(country.value);

  } catch (error) {
    console.error('Error fetching country details:', error);
  }

}

onMounted(() => {
  fetchCountryDetails(route.params.alpha3Code);
});

watch(() => route.params.alpha3Code, (newVal, oldVal) => {
  if(newVal !== oldVal) {
    fetchCountryDetails(newVal);
  }
});
</script>
