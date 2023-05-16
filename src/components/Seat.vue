<template>
  <l-marker
    @click="handleClick"
    :key="seat.id"
    :lat-lng="[seat.center[0], seat.center[1] - 0.1]"
  >
    <l-icon class-name="hidden">
      <div class="w-12">
        {{ seat.id }}
      </div>
    </l-icon>
  </l-marker>
  <l-circle
    @click="handleClick"
    class-name="cursor-pointer z-10"
    :key="seat.id"
    :lat-lng="seat.center"
    :radius="seat.radius"
    :color="seatColor"
    :fill-color="seatColor"
    :fill-opacity="1"
  />
</template>

<script setup lang="ts">
import { LCircle, LMarker, LIcon } from "@vue-leaflet/vue-leaflet";
import { PropType, computed } from "vue";
import ISeat from "../types/ISeat";

const { seat, selectedSeats } = defineProps({
  seat: {
    type: Object as PropType<ISeat>,
    required: true,
  },
  selectedSeats: {
    type: Array as PropType<ISeat[]>,
    required: true,
  },
});

const emit = defineEmits(["select"]);

const handleClick = () => {
  emit("select", seat);
};

const seatColor = computed(() => {
  if (selectedSeats.includes(seat)) {
    return "red";
  }
  return "green";
});
</script>

<style scoped></style>
