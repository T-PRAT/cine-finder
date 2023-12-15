<script setup>
import { MapboxMap, MapboxMarker } from "@studiometa/vue-mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

const props = defineProps({
  location: Object,
  cinemasList: Array,
});
</script>

<template>
  <div class="col-span-3 mx-12 pt-6">
    <MapboxMap
      class="absolute w-[115vh] h-[80vh] border border-gray-300 rounded-md shadow-md overflow-hidden"
      ref="mapboxMap"
      :zoom="9"
      :center="location"
      mapStyle="mapbox://styles/mapbox/streets-v11"
      access-token="pk.eyJ1IjoidHByYXQiLCJhIjoiY2xxNnJ1NW13MG9lMDJrcDF2M3d0N3J1MiJ9.dVKhWLavKLJXFb_S3nFBqw"
    >
      <MapboxMarker
        v-if="cinemasList.length > 0"
        v-for="cinema in cinemasList"
        :key="cinema.id"
        :lng-lat="[cinema.longitude, cinema.latitude]"
        :offset="[-12, -24]"
        :draggable="false"
      />
    </MapboxMap>
  </div>
</template>
