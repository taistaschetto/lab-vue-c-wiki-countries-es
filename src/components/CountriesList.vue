<template>
  <router-view />
  <div class="countries-list">
    <ul class="list-group">
      <li
        class="list-group-item"
        v-for="country in countries"
        :key="country.alpha3Code"
      >
        <router-link :to="`/list/${country.alpha3Code}`">{{
          country.name.common
        }}</router-link>
      </li>
    </ul>
    
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const countries = ref([]);

onMounted(async () => {
  const response = await fetch(
    "https://ih-countries-api.herokuapp.com/countries"
  );
  if (response.ok) {
    countries.value = await response.json();
  } else {
    console.error("Failed to fetch countries:", response.status);
  }
});
</script>
