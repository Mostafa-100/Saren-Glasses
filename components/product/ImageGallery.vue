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
  <div class="relative w-full max-w-3xl mx-auto overflow-hidden">
    <!-- Image -->
    <img
      :src="props.images[currentIndex].src"
      :alt="props.images[currentIndex].alt || 'Slide Image'"
      :class="`object-cover rounded-lg ${props.imageSizeClasses ? props.imageSizeClasses : 'w-full h-auto'}`"
    />

    <!-- Arrows -->
    <button
      class="absolute top-1/2 left-4 transform -translate-y-1/2 text-3x rounded-full p-2"
      @click="prev"
    >
      ‹
    </button>
    <button
      class="absolute top-1/2 right-4 transform -translate-y-1/2 text-3xl p-2"
      @click="next"
    >
      ›
    </button>
  </div>
</template>
