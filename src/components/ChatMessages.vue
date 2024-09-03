<template>
    <div class="overflow-y-auto max-h-full md:h-96 text-black flex-grow">
      <div v-if="loading" class="flex flex-col leading-1.5 p-1 mr-5 ml-1 rounded-e-xl rounded-es-xl bg-gray-300 mt-1">
        <p class="text-xs font-normal px-1 py-2.5 text-black">{{ loadingText }}</p>
      </div> 
      <div v-else v-for="message in messages.filter(message => !message.hidden)" :key="message.content" class="flex flex-col leading-1.5 p-1 bg-gray-300 mt-1" :class="{ 'mr-5 ml-1 rounded-e-xl rounded-es-xl': message.role!='user', 'ml-5 mr-1 rounded-xl rounded-br-none': message.role=='user' }">
        <p class="text-xs font-normal px-1 py-2.5 text-black">{{ message.content }}</p>
      </div>
      <div v-if="typing" class="flex flex-col leading-1.5 p-1 mr-5 ml-1 rounded-e-xl rounded-es-xl bg-gray-300 mt-1">
        <p class="animate-pulse text-xs font-normal px-1 py-2.5 text-black">Typing...</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  const props = defineProps({
    messages: Array,
    loading: Boolean,
    typing: Boolean,
    loadingText: String
  });
  </script>