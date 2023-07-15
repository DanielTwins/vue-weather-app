<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full border-b bg-transparent focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const mapboxAPIKey =
  "pk.eyJ1IjoiZGFuaWVsLW5hc2VyaSIsImEiOiJjbGsyb2wyeGswajkzM2NtcHNhbTlsZDgzIn0.eZYdYuM3mg8pJ4D_jh-3kA";
const mapboxSearchResults = ref(null);
console.log(
  "🚀 ~ file: HomeView.vue:21 ~ mapboxSearchResults:",
  mapboxSearchResults
);

const searchQuery = ref("");
const queryTimeout = ref(null);
// state variable for holding result of a lazy search
// through a setTimeout, meaning after user stoped typing

// fetch API result from geolocation
const getSearchResults = () => {
  // clear the setTimeout each time getSearchResults is ran
  clearTimeout(queryTimeout.value);

  queryTimeout.value = setTimeout(async () => {
    if (searchQuery !== "") {
      // ensure that if there is a value in searchQuery var
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}`
      );
      mapboxSearchResults.value = result.data.features;
      console.log(mapboxSearchResults.value);
      return;
    }
    // set null if no search query value
    mapboxSearchResults.value = null;
  }, 300); // the amount of time waiting before invoking the call back func and the code inside
};
</script>
