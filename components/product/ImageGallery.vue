<script setup lang="ts">
const props = defineProps<{
  images: Array<{ src: string; alt?: string }>;
  imageSizeClasses: String;
}>();

const currentIndex = ref(0);

const next = () => {
  currentIndex.value = (currentIndex.value + 1) % props.images.length;
};

const prev = () => {
  currentIndex.value =
    currentIndex.value === 0 ? props.images.length - 1 : currentIndex.value - 1;
};
</script>

<template>
  <div class="relative w-full max-w-3xl mx-auto overflow-hidden group">
    <!-- Image -->
    <img
      :src="props.images[currentIndex].src"
      :alt="props.images[currentIndex].alt || 'Slide Image'"
      :class="`object-cover ${props.imageSizeClasses ? props.imageSizeClasses : 'w-full h-auto'}`"
    />

    <!-- Arrows -->
    <button
      class="absolute top-1/2 left-4 transform -translate-y-1/2 text-3xl rounded-full p-2 opacity-0 transition duration-300 group-hover:opacity-100"
      @click="prev"
    >
      ‹
    </button>
    <button
      class="absolute top-1/2 right-4 transform -translate-y-1/2 text-3xl rounded-full p-2 opacity-0 transition duration-300 group-hover:opacity-100"
      @click="next"
    >
      ›
    </button>
  </div>
</template>
