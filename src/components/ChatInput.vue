<template>
    <div class="flex items-center py-2 rounded-b-xl bg-gray-300">
      <textarea 
        @keydown.enter.prevent="sendMessage"
        id="chat" rows="1" class="block mx-2 p-2.5 w-full h-[92px] text-sm text-black bg-white rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500" placeholder="Your message..."></textarea>
      <button 
        @click="sendMessage"
        class="inline-flex justify-center p-2 text-black rounded-full cursor-pointer hover:bg-green-500 hover:text-white">
        <svg class="w-5 h-5 rotate-90 rtl:-rotate-90" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 18 20">
          <path d="m17.914 18.594-8-18a1 1 0 0 0-1.828 0l-8 18a1 1 0 0 0 1.157 1.376L8 18.281V9a1 1 0 0 1 2 0v9.281l6.758 1.689a1 1 0 0 0 1.156-1.376Z"/>
        </svg>
        <span class="sr-only">Send message</span>
      </button>
    </div>
  </template>
  
  <script setup>
  import { defineEmits } from 'vue';
  
  const emit = defineEmits(['sendMessage']);
  
  const sendMessage = () => {
    emit('sendMessage');
  }
  </script>