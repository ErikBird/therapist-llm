<script setup>
import { onMounted, ref } from 'vue';
import * as webllm from "@mlc-ai/web-llm";
import { CreateWebWorkerMLCEngine } from "@mlc-ai/web-llm";

// Callback function to update model loading progress
const initProgressCallback = (initProgress) => {
  loadingText.value = initProgress.text;
}
const selectedModel = "Llama-3.1-8B-Instruct-q4f32_1-MLC";

let engine = null;
const loading = ref(true);
const loadingText = ref("");
const messages = ref([
  { role: "system", content: "You are a helpful AI assistant." },
  { role: "user", content: "Hello!" },
]);
onMounted( async () => {
  loading.value = true;
  engine = await CreateWebWorkerMLCEngine(
    new Worker(
      new URL("./worker.ts", import.meta.url), 
      {
        type: "module",
      }
    ),
    selectedModel,
    { initProgressCallback }, // engineConfig
  );
  loading.value = false;
  
  // Convert messages to a plain object
  const plainMessages = JSON.parse(JSON.stringify(messages.value));
  const reply = await engine.chat.completions.create({ "messages": plainMessages });
  messages.value.push(reply.choices[0].message );
  console.log(reply.choices[0].message);
  console.log(reply.usage);
})

</script>

<template>
  <div v-if="loading"> 
    {{loadingText}}
  </div> 
  <div v-else>
    <div v-for="message in messages" :key="message.content">
      {{ message.content }}
    </div>
  </div>

</template>