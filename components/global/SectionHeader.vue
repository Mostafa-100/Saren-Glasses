<script setup>
defineProps({
  content: String,
  otherClass: String,
});
</script>

<template>
  <h2 :class="`text-xl font-semibold ${otherClass ? otherClass : ''}`">
    {{ content }}
  </h2>
</template>
