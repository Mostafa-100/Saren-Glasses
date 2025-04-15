<script setup lang="ts">
const { $settings } = useNuxtApp();
const { location } = $settings.sections;
</script>

<template>
  <div class="py-36" :style="{ backgroundColor: location.background_color }">
    <div class="container mx-auto">
      <div class="flex flex-col justify-center items-center gap-4">
        <GlobalSectionHeader :content="location.content.header" />
        <img
          class="max-w-[600px]"
          :src="location.image ? location.image.src : ''"
        />
        <div class="text-sm max-w-[400px] text-center">
          {{ location.content.description }}
        </div>
        <GlobalSectionLink
          :label="location.button.text"
          :link="location.button.link"
        />
      </div>
    </div>
  </div>
</template>
