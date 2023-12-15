<script setup>
import { ref } from "vue";
const baseUrl = "https://data.culture.gouv.fr/api/explore/v2.1/catalog/datasets/etablissements-cinematographiques/";
const inputValue = ref("");
const cinemasList = ref([]);

const sortByDistance = (cinemas, location) => {
  const sortedCinemas = cinemas.sort((a, b) => {
    const aDistance = Math.sqrt(Math.pow(a.longitude - location.lng, 2) + Math.pow(a.latitude - location.lat, 2));
    const bDistance = Math.sqrt(Math.pow(b.longitude - location.lng, 2) + Math.pow(b.latitude - location.lat, 2));
    a.distance = aDistance;
    b.distance = bDistance;
    return aDistance - bDistance;
  });
  return sortedCinemas;
};

const fetchCinemas = async (location) => {
  try {
    const response = await fetch(
      baseUrl + `records?where=within_distance(geolocalisation%2C%20geom'POINT(${location.lng}%20${location.lat})'%2C%2010km)&limit=50`
    );
    const data = await response.json();
    cinemasList.value = sortByDistance(data.results, location);
  } catch (error) {
    console.error(error);
  }
};

const getUserLocation = async () => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      ({ coords }) => {
        const userLocation = {
          lat: coords.latitude.toFixed(2),
          lng: coords.longitude.toFixed(2),
        };
        fetchCinemas(userLocation);
      },
      (error) => {
        console.error(`Error getting user's location: ${error.message}`);
      }
    );
  } else {
    console.error("Geolocation is not supported by your browser");
  }
};

const getInputLocation = async () => {
  if (!inputValue.value) return;
  try {
    const response = await fetch(`https://api-adresse.data.gouv.fr/search/?q=${inputValue.value}&type=municipality&autocomplete=1`);
    const data = await response.json();
    const location = {
      lat: data.features[0].geometry.coordinates[1],
      lng: data.features[0].geometry.coordinates[0],
    };
    fetchCinemas(location);
  } catch (error) {
    console.error(error);
  }
};
</script>

<template>
  <div class="col-span-2 m-12">
    <h2 class="text-2xl">Rechercher un cinema</h2>
    <div class="flex space-x-2 py-2">
      <input v-model="inputValue" placeholder="Entrer la localisation" class="input input-bordered w-full max-w-xs" />
      <button class="btn btn-secondary" type="button" @click="getInputLocation">
        <svg class="h-6 fill-none stroke-current" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
          <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z"></path>
        </svg>
      </button>
      <button class="btn btn-primary" type="button" @click="getUserLocation">
        <svg class="h-6 fill-none stroke-current" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
          <path stroke-linecap="round" stroke-linejoin="round" d="M15 10.5a3 3 0 11-6 0 3 3 0 016 0z"></path>
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M19.5 10.5c0 7.142-7.5 11.25-7.5 11.25S4.5 17.642 4.5 10.5a7.5 7.5 0 1115 0z"
          ></path>
        </svg>
      </button>
    </div>

    <div class="divider" />
    <div>
      <ul v-for="cinema in cinemasList" :key="cinema.id" class="my-4">
        <li class="bg-base-200 p-2 shadow rounded-xl">
          <div class="flex justify-between">
            <p class="font-bold">{{ cinema.nom }}</p>
            <p>{{ cinema.commune }}</p>
          </div>
          <div class="flex justify-between">
            <div>
              <p>ecrans: {{ cinema.ecrans }}</p>
              <p>fauteuils: {{ cinema.fauteuils }}</p>
            </div>
            <p>distance: {{ (cinema.distance * 100).toFixed(1) }}km</p>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>
