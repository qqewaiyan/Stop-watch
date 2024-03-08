<script setup lang="ts">
import { ref } from "vue";

const State = ref<"stopped" | "running" | "paused">("stopped");
const interval = ref<number | undefined>(undefined);
const timeElapsed = ref(0);

function start() {
  State.value = "running";
  interval.value = setInterval(() => {
    timeElapsed.value++;
  }, 10);
}

function pause() {
  State.value = "paused";
  clearInterval(interval.value);
  interval.value = undefined;
}

function reset() {
  timeElapsed.value = 0;
  clearInterval(interval.value);
  interval.value = undefined;
  start();
}

function formatTime(timeElapsed: number) {
  const minutes = `0${Math.floor(timeElapsed / 6000)}`.slice(-2);
  const seconds = `0${Math.floor((timeElapsed % 6000) / 100)}`.slice(-2); 
  const milliseconds = `0${Math.floor(timeElapsed % 100)}`.slice(-2); 
  return `${minutes} : ${seconds} : ${milliseconds}`;
}
</script>

<template>
  <div>
    <div style="margin-bottom: 10px">{{ formatTime(timeElapsed) }}</div>
    <button v-if="State === 'stopped'" @click="start() " style="">Start</button>
    <button v-if="State === 'running'" @click="pause()">Pause</button>
    <button v-if="State === 'paused'" @click="start()">Resume</button>
    <button @click="reset()">Reset</button>
  </div>
</template>

<style scoped>
</style>
