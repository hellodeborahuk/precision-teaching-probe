<template>
  <div class="bg-white rounded-xl shadow-md p-6 text-left border border-gray-100">
    <h2 class="text-lg font-semibold text-gray-800 mb-2">Example Component</h2>
    <p class="text-sm text-gray-500 mb-4">A sample BaseCard component styled with Tailwind.</p>
    <button @click="count++"
      class="w-full py-2 rounded-lg bg-indigo-500 text-white text-sm font-medium hover:bg-indigo-600 active:scale-95 transition-all">
      Clicked {{ count }} {{ count === 1 ? 'time' : 'times' }}
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>
