<script setup lang="ts">
const props = defineProps({
  alt: {
    type: String,
    default: "No alt found",
  },
  src: {
    type: String,
    default:
      "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4",
  },
  property: {
    type: String,
    default: null,
  },
  default: {
    type: String,
    default: null,
  },
  imgClass: {
    type: String,
    default: null,
  },
});
</script>

<template>
  <img :src="props.src" :alt="props.alt" :class="imgClass" />
</template>
