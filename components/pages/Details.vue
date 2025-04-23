<script setup lang="ts">
defineProps({
  item: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <!--  -->
  <div class="container lg:px-[50px] mx-auto">
    <div class="flex flex-col gap-6">
      <!--  -->
      <h2 v-if="item.title.length > 0" class="text-4xl text-center uppercase">
        {{ item.title }}
      </h2>
      <!--  -->

      <!--  -->
      <div v-if="item.excerpt.length > 0" class="relative overflow-hidden">
        <!--  -->
        <div class="absolute inset-0 w-2 bg-[#F1F1F1]"></div>
        <!--  -->
        <div class="p-6 md:p-8 italic">
          <!--  -->
          <h3 class="text-sm md:text-base font-normal">{{ item.excerpt }}</h3>
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->

      <!--  -->
      <div
        v-if="item.content.length > 0"
        id="description"
        class="custom-page-content text-[16px] leading-[1.8] *:pb-4"
        v-html="item.content"
      ></div>
      <!--  -->
    </div>
    <!--  -->
  </div>
</template>

<style>
.custom-page-content h2 {
  text-transform: uppercase;
  font-size: 1.5rem;
  padding-bottom: 6px;
}

.custom-page-content h3 {
  text-transform: uppercase;
  font-size: 1.2rem;
  padding-bottom: 6px;
}
</style>
