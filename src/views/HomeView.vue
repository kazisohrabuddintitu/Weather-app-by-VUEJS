
<template>
  
  <main class="container text-white">
    <div class="pt-4 mb-4 relative">
      <input type="text" 
      v-model="searchQuery"
      @input="getSearchResults"
      placeholder="Search for a city or state"
      class="py-2 px-1 w-full bg-transparent border-b
       focus:border-weather-secondary focus:outline-none 
       focus:shadow-[0px_1px_0_0_#004E71]"
       />
       <ul class="absolute bg-weather-secondary text-white
        w-full
       shadow-md py-2 px-1 top-[66px]"
       v-if="openApiSearchResults"
       >
       <li v-for="searchResult in openApiSearchResults" :key="searchResult.id"
       class="py-2 cursor-pointer"
       @click="previewCity(searchResult)"
       >
           {{ searchResult.place_name }}
       </li>
        </ul>
    </div>
  </main>
  
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";

const openAPIKey= "bcb3bda9a7b598abac00428bbd4956a1";

const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";


const searchQuery= ref("");
const queryTimeout=ref(null);
const openApiSearchResults= ref(null);


const getSearchResults= ()=> {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async()=>{
    if (searchQuery !==""){
      const result= await axios.get(
        'https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&units=imperial&APPID=${openAPIKey}');
        openApiSearchResults.value= result.data.features;
        
        return;  
    }
    openApiSearchResults.value= null;
  }, 300);
};
/*
const mapboxSearchResults = ref(null);
const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
      );
      mapboxSearchResults.value = result.data.features;
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};*/
</script>

