<script setup>
import { defineProps} from 'vue'

let props = defineProps({
    results: Object
})


</script>

<template>
    <div>
        <p v-for="result in props.results" :key="result.id">
            {{ result.name }} ... {{ result.percentage }}%
        </p>
    </div>
</template>
