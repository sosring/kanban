<script setup lang="ts">

  const showkeyStroke = ref(true)
</script>

<template>

  <div class="p-10 h-[100vh] bg-teal-600 overflow-auto relative">
    <span class="flex items-center justify-between mb-10 gap-4">
      <h1 class="text-4xl text-white"> Kanban Board </h1>

      <button class="text-xs rounded font-bold p-2 shadow-sm"
       :class="showkeyStroke ? 'bg-black text-gray-100 ' : 
       'bg-gray-300 text-gray-600'"
       @click="showkeyStroke = !showkeyStroke">
        Key Stroke: {{ showkeyStroke ? 'ON' : 'OFF' }}
      </button>
    </span>

    <TrelloBoard />

    <KeyListener v-if="showkeyStroke" />
  </div>
</template>
