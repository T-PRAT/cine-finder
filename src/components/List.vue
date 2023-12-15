<script setup>
const props = defineProps({
  cinemasList: Array,
});
</script>

<template>
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
</template>
