<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])

const seachTerm = reactive({
    query: '',
    timeout: null,
    results: null
});

const handleSearch = () => {
    clearTimeout(seachTerm.timeout)

    seachTerm.timeout = setTimeout(async () => {
        if (seachTerm.query !== '') {
            const res = await fetch(
                `https://api.weatherapi.com/v1/search.json?key=94b17c8d90154b9da4292007243101&q=${seachTerm.query}`
            )

            const data = await res.json()
            seachTerm.results = data
        } else {
            seachTerm.results = null
        }

    }, 500);
};

const getWeather = async (id) => {
    const res = await fetch(
        `https://api.weatherapi.com/v1/forecast.json?key=94b17c8d90154b9da4292007243101&q=id:${id}&days=3&aqi=no&alerts=no`
    )

    const data = await res.json()
    emit('place-data', data)

    seachTerm.query = ''
    seachTerm.results = null
}

</script>

<template>
    <div>
        <!-- search field -->
        <form>
            <div class="bg-white border border-indigo-600/30 rounded-3xl shadow-lg flex items-center">
                <i class="fa-solid fa-magnifying-glass p-6 text-indigo-600/30"></i>
                <input type="text" placeholder="Search for a place"
                    class="rounded-r-3xl p-6 border-0 outline-0 w-full"
                    v-model="seachTerm.query" @input="handleSearch" />
            </div>
        </form>

        <!-- search suggestions -->
        <div class="bg-white my-2 rounded-lg shadow-lg">
            <div v-if="seachTerm.results !== null">
                <div v-for="place in seachTerm.results" :key="place.id">
                    <button @click="getWeather(place.id)"
                        class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
                        {{ place.name }}, {{ place.region }}, {{ place.country }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>