<template>
    <div>
        <img v-if="error" class="hidden md:block absolute -right-72 top-0 rounded-full w-44 h-44" src="/src/assets/annoyed.jpeg" alt="annoyed avatar">
        <img v-else-if="loading" class="hidden md:block absolute -right-72 top-0 rounded-full w-44 h-44" src="/src/assets/waiting.jpeg" alt="waiting avatar">
        <img v-else-if="typing" class="hidden md:block absolute -right-72 top-0 rounded-full w-44 h-44" src="/src/assets/thinking.jpeg" alt="thinking avatar">
        <img v-else-if="!typing && !loading" class="hidden md:block absolute -right-72 top-0 rounded-full w-44 h-44" src="/src/assets/listening.jpeg" alt="listening avatar">
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  const props = defineProps({
    error: Boolean,
    loading: Boolean,
    typing: Boolean
  });
  </script>