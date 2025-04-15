<script setup>
const { $settings } = useNuxtApp();
const { selections } = $settings.sections;

console.log(selections);
</script>

<template>
  <div class="relative py-14 flex flex-col justify-center items-center">
    <img
      :src="selections.image.src"
      class="absolute -z-10 top-0 left-0 w-full h-full object-cover brightness-50"
    />
    <div class="text-center text-white max-w-[425px]">
      <GlobalSectionHeader :content="selections.content.title" />
      <p class="text-sm my-6">{{ selections.content.description }}</p>
    </div>
    <div v-for="(product, index) in selections.products" :key="index">
      <div>
        <ElementProduct
          :item="product"
          textColorClass="text-white"
          imageSizeClasses="w-[400px]"
        />
      </div>
    </div>
  </div>
</template>
