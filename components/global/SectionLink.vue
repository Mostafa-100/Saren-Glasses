<script setup>
defineProps({
  label: String,
  link: String,
});
</script>
<template>
  <NuxtLink
    :to="link"
    class="text-sm underline underline-offset-3 order-3 mt-3 lg:mt-0 flex items-center"
  >
    <span>{{ label }}</span>
    <Icon name="solar:arrow-right-linear" />
  </NuxtLink>
</template>
