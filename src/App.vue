<template>

  <div class="flex mx-auto w-screen h-screen justify-center">
    <div class="relative flex flex-col justify-end justify-items-end h-screen md:h-[520px] w-screen md:w-[250px] border border-4 md:border-black md:rounded-2xl bg-gray-50 shadow-2xl">
      <PhoneNotch />
      <ChatMessages :messages="messages" :loading="loading" :typing="typing" :loadingText="loadingText" />
      <ChatInput @sendMessage="sendMessage" />
      <TherapistAvatar :loading="loading" :typing="typing" :error="hasError" />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import * as webllm from "@mlc-ai/web-llm";
import { CreateWebWorkerMLCEngine } from "@mlc-ai/web-llm";
import PhoneNotch from './components/PhoneNotch.vue';
import ChatMessages from './components/ChatMessages.vue';
import ChatInput from './components/ChatInput.vue';
import TherapistAvatar from './components/TherapistAvatar.vue';

const initProgressCallback = (initProgress) => {
  loadingText.value = initProgress.text;
}
const selectedModel = "Llama-3.2-3B-Instruct-q4f32_1-MLC";

let engine = null;
const loading = ref(false);
const typing = ref(false);
const hasError = ref(false);
const loadingText = ref("The APP is starting up...");
const messages = ref([
  { role: "system", content: "You are a helpful AI assistant that acts like a therapist. You draw on extensive research into existential thought to guide conversations and ensure that the principles and practices of existential therapy are maintained in their interactions. This approach provides a solid framework to address the user's concerns and encourage deep and meaningful engagement with the topics that interest them.", hidden: true },
  { role: "user", content: "Hello!", hidden: true },
]);

onMounted(async () => {
  loading.value = true;
  try {
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
  } catch (error) {
    loadingText.value = error;
    hasError.value = true;
    return;
  }
  await generateMessage();
})

const sendMessage = async () => {
  const message = document.getElementById("chat").value;
  document.getElementById("chat").value = "";
  messages.value.push({ role: "user", content: message, hidden: false });
  await generateMessage();
}

const generateMessage = async () => {
  const plainMessages = JSON.parse(JSON.stringify(messages.value));
  typing.value = true;
  const reply = await engine.chat.completions.create({ "messages": plainMessages });
  typing.value = false;
  messages.value.push(reply.choices[0].message);
}
</script>
