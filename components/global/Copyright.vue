<script setup lang="ts">
const { $settings } = useNuxtApp();

const year = new Date().getFullYear();

const copyright = $settings.sections.copyright;
const description = copyright.text;

const bgColor = copyright.colors.background_color;
const textColor = copyright.colors.text_color;
</script>

<template>
  <!--  -->
  <div class="flex flex-col text-center md:text-start">
    <!--  -->
    <div
      class="border-t"
      :style="{ backgroundColor: bgColor, color: textColor }"
    >
      <!--  -->
      <div v-if="description.length > 0" class="container py-6">
        <!--  -->
        <div
          class="text-sm font-light"
          v-html="description.replace(/__YEAR__/gi, year)"
        ></div>
        <!--  -->
      </div>
      <!--  -->
    </div>

    <!--  -->
    <div class="flex items-center justify-center py-2 bg-gray-800">
      <!--  -->
      <small class="text-base font-normal text-white">
        Made by
        <!--  -->
        <a
          class="font-bold text-blue-600"
          href="//www.storeino.com/"
          target="_blank"
          >Storeino.com</a
        >
        <!--  -->
      </small>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
