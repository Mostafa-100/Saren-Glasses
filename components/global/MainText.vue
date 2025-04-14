<script setup>
defineProps({
  content: String,
  otherClass: String,
});
</script>

<template>
  <p :class="`text-sm ${otherClass ? otherClass : ''}`">
    {{ content }}
  </p>
</template>
