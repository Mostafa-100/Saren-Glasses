<template>
  <div
    class="min-h-screen flex items-center justify-center bg-[#F1F1F1] text-gray-800 p-6"
  >
    <div class="text-center">
      <h1 class="text-6xl font-bold mb-4">Oops!</h1>
      <p class="text-xl mb-2">An error occurred</p>
      <p class="text-md text-gray-600 mb-6">
        <span v-if="error.statusCode">Error {{ error.statusCode }}</span>
        <span v-else>Something went wrong.</span>
      </p>
      <NuxtLink
        to="/"
        class="inline-block bg-black text-white px-6 py-3 rounded-xl hover:bg-gray-800 transition"
      >
        Go Back Home
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
defineProps({
  error: {
    type: Object,
    required: true,
  },
});
</script>

<style scoped>
h1 {
  color: #000;
}
</style>
