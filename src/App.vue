<script setup>
import { ref } from 'vue';
import InputSearch from './components/InputSearch.vue';
import WeatherCard from './components/WeatherCard.vue';

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  if (confirm('Are you sure you want to delete this place?') === false) return

  places.value = places.value.filter((place) => place.location.name !== name)
}
</script>

<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6 font-bold text-xl">
      {{
        new Date().toLocaleDateString('en-us', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>

    <!-- Search -->
    <div>
      <InputSearch @place-data="addPlace" />
    </div>

    <!-- Place Weather Cards -->
    <div class="grid md:grid-cols-2 gap-8 mt-8">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" @delete-place="deletePlace"/>
      </div>
    </div>
  </main>
</template>