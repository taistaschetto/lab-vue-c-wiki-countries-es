<template>
    <div class="countries-list">
      <ul class="list-group">
        <li class="list-group-item" v-for="country in countries" :key="country.alpha3Code">
          <router-link :to="`/list/${country.alpha3Code}`">{{ country.name }}</router-link>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  // Example data, replace with actual data fetching logic later
  const countries = ref([
    { name: 'France', alpha3Code: 'FRA' },
    { name: 'Germany', alpha3Code: 'DEU' },
    // Add more countries as needed
  ]);
  
  // You can add composables here to fetch real data instead of using static data
  </script>