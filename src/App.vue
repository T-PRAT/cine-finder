<script setup>
import Search from "./components/Search.vue";
import Map from "./components/Map.vue";
</script>

<template>
  <header class="m-12">
    <h1 class="text-4xl font-mono font-bold">Cine Finder</h1>
  </header>
  <main class="m-y-12 font-mono grid grid-cols-5">
    <Search />
    <Map />
  </main>
</template>
