<template>
  <div style="height: 600px; width: 1200px">
    <l-map
      :useGlobalLeaflet="false"
      ref="map"
      v-model:zoom="zoom"
      :center="[4, 11]"
      :options="{
        minZoom: 5,
        maxZoom: 7,
        doubleClickZoom: false,
      }"
    >
      <!-- SCREEN -->
      <l-rectangle
        :bounds="[
          [-3, 22],
          [-5, 0],
        ]"
        color="#1C82AD"
        :fill-color="'#1C82AD'"
        :fill-opacity="1"
      ></l-rectangle>
      <l-marker :lat-lng="[-3.5, 10.5]">
        <l-icon class-name="hidden">
          <div class="text-3xl text-white">SCREEN</div>
        </l-icon>
      </l-marker>
      <!-- MOVIE POSTER -->
      <l-image-overlay
        url="https://m.media-amazon.com/images/M/MV5BMDgxOTdjMzYtZGQxMS00ZTAzLWI4Y2UtMTQzN2VlYjYyZWRiXkEyXkFqcGdeQXVyMTkxNjUyNQ@@._V1_FMjpg_UX1000_.jpg"
        :bounds="[
          [0, 25],
          [11, 32],
        ]"
      ></l-image-overlay>
      <!-- Seats -->
      <Seat
        @select="handleSelect"
        v-for="seat in seats"
        :seat="seat"
        :selectedSeats="selectedSeats"
      ></Seat>
      <!-- SELECTED SEATS -->
      <l-control position="topright"
        ><div class="bg-white px-4 py-2 justify-center items-center">
          <h2 class="text-2xl font-bold">Selected Seats:</h2>
          <li class="text-xl" v-for="seat in selectedSeats" :key="seat.id">
            Seat {{ seat.id }}
          </li>
        </div>
      </l-control>
    </l-map>
  </div>
</template>

<script setup lang="ts">
import "leaflet/dist/leaflet.css";
import {
  LMap,
  LRectangle,
  LMarker,
  LIcon,
  LImageOverlay,
  LControl,
} from "@vue-leaflet/vue-leaflet";
import { onMounted, ref } from "vue";
import Seat from "./Seat.vue";
import ISeat from "../types/ISeat";

const columns = 12;
const rows = 11;

const zoom = ref(5);
const seats = ref<ISeat[]>([]);
const selectedSeats = ref<ISeat[]>([]);

const generateSeats = () => {
  const seats = [];
  for (let i = 0; i <= rows; i++) {
    for (let j = 0; j < columns; j++) {
      seats.push({
        id: `${String.fromCharCode(65 + i)}${j + 1}`,
        center: [i, j * 2],
        radius: 40000,
      });
    }
  }
  return seats;
};

const handleSelect = (seat: ISeat) => {
  if (selectedSeats.value.includes(seat)) {
    selectedSeats.value.splice(selectedSeats.value.indexOf(seat), 1);
  } else {
    selectedSeats.value.push(seat);
  }
};

onMounted(() => {
  seats.value = generateSeats();
  console.log(seats.value);
});
</script>

<style scoped>
.leaflet-container {
  background: #252526;
  outline: 0;
}
</style>
