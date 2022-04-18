<script setup>
import { defineProps} from 'vue'
const props = defineProps({
    results: Object,
    b64Img: String
})



</script>

<template>
    <div>
        <img :src="props.b64Img" alt="" accept="image/*">
    </div>
    <h1>WhatsCat says your cat is a...</h1>
    <div class="result-container" v-for="result in props.results" :key="result.id">
        <p class="breed-item">{{ result.name }}</p>
        <p class="separator-item">---------</p>
        <p class="percentage-item">{{ result.percentage }}%</p>
    </div>
</template>

<style scoped>
.result-container {
    display: grid;
    grid-template-columns: 12vw 12vw 12vw;
}

.breed-item {
    justify-self: start;
}

.separator-item {
    justify-self: center;
}

.percentage-item {
    justify-self: end;
}


img {
  padding: 0;
  margin: 0;
  border-radius: 2vw;
  width: min(64vw, 640px);
  height: min(36vw, 360px);
}
</style>