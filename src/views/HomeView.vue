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
      <!-- v-if to show this ul section if there is a search results fetched -->
      <ul
        class="absolute top-[66px] w-full bg-weather-secondary text-white py-2 px-1"
        v-if="mapboxSearchResults"
      >
        <p v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p v-if="!serverError && mapboxSearchResults.length === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
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
const searchError = ref(null);

// fetch API result from geolocation
const getSearchResults = () => {
  // clear the setTimeout each time getSearchResults is ran
  clearTimeout(queryTimeout.value);

  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      // ensure that if there is a value in searchQuery var

      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
        console.log(mapboxSearchResults.value);
      } catch {
        searchError.value = true;
      }
      return;
    }
    // set null if no search query value
    mapboxSearchResults.value = null;
  }, 300); // the amount of time waiting before invoking the call back func and the code inside
};
</script>
