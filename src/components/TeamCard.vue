<script setup>
import { ref, onMounted } from 'vue';

// props
const props = defineProps({
  name: String,
  
})
</script>

<template>
<div class="name">
    <h1>{{ name }}</h1>
</div>

</template>

<style>

</style>