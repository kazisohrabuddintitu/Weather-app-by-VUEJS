
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
       
    </div>
  </main>
  
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";

// const previewCity = (searchResult) =>{
//   console.log(searchResult);
// };


const searchQuery= ref("");
const queryTimeout=ref(null);
const openApiSearchResults= ref(null);


const getSearchResults= ()=> {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async()=>{
    if (searchQuery !==""){
      try{      
        
        const result= await axios.get(
        "https://api.openweathermap.org/data/2.5/forecast?q="+searchQuery.value+"&exclude={part}&appid=bcb3bda9a7b598abac00428bbd4956a1&units=imperial");
           
        openApiSearchResults.value= result.data;
        console.log(openApiSearchResults);
      }
      catch(err){
        console.log(err);
      }    
        return;  
    }
    openApiSearchResults.value= null;
  }, 300);
};

</script>

