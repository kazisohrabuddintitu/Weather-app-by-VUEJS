
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

    <div class="weather loading py-8 grid
     place-items-center max-w-screen-md
      bg-weather-white text-black">
      <h2 class="city">Weather in </h2>
      <h1 class="temp">°C</h1>
      <div class="flex">
        <img src="https://openweathermap.org/img/wn/04n.png" alt="" class="icon" />
        <div class="description">Cloudy</div>
      </div>
      <div class="humidity">Humidity: %</div>
      <div class="wind">Wind speed:  km/h</div>
    </div>
  </main>
  
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";


const searchQuery= ref("");
const queryTimeout=ref(null);



const getSearchResults= ()=> {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async()=>{
    if (searchQuery !==""){
      try{      
        
        const result= await axios.get(
        "https://api.openweathermap.org/data/2.5/weather?q="+searchQuery.value+"&units=metric&appid=bcb3bda9a7b598abac00428bbd4956a1");
           
        displayWeather(result.data);
      }
      catch(err){
        console.log(err);
      }    
        return;  
    }
  }, 300);
  function displayWeather(data){
    const { name } = data;
    const { icon, description } = data.weather[0];
    const { temp, humidity } = data.main;
    const { speed } = data.wind;
    document.querySelector(".city").innerText = "Weather in " + name;
    document.querySelector(".icon").src =
      "https://openweathermap.org/img/wn/" + icon + ".png";
    document.querySelector(".description").innerText = description;
    document.querySelector(".temp").innerText = temp + "°C";
    document.querySelector(".humidity").innerText =
      "Humidity: " + humidity + "%";
    document.querySelector(".wind").innerText =
      "Wind speed: " + speed + " km/h";
    document.querySelector(".weather").classList.remove("loading");
  }
};

</script>

