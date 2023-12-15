<script setup>
import { ref } from "vue";
import { MapboxMap, MapboxMarker } from "@studiometa/vue-mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

const mapboxMap = ref();
</script>

<template>
  <div class="col-span-3">
    <MapboxMap
      class="absolute w-[700px] h-[500px]"
      ref="mapboxMap"
      access-token="pk.eyJ1IjoidHByYXQiLCJhIjoiY2xxNnJ1NW13MG9lMDJrcDF2M3d0N3J1MiJ9.dVKhWLavKLJXFb_S3nFBqw"
    >
    </MapboxMap>
  </div>
</template>
