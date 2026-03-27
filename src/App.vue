<template>
  <div class="min-h-screen bg-blue-200">
    <!-- <nav class="bg-white shadow-sm border-b border-gray-200">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16 items-center">
          <span class="text-xl font-bold text-gray-900">Vue3 + Tailwind</span>
          <div class="flex gap-4">
            <RouterLink to="/" class="text-gray-600 hover:text-gray-900 text-sm font-medium transition-colors">Home</RouterLink>
            <RouterLink to="/about" class="text-gray-600 hover:text-gray-900 text-sm font-medium transition-colors">About</RouterLink>
          </div>
        </div>
      </div>
    </nav> -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
      <RouterView />
    </main>
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>
