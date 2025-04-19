<script setup lang="ts">
defineProps({
  texts: {
    type: Object,
    default: () => ({
      empty_text: "",
    }),
  },
  buttons: {
    type: Object,
    default: () => ({
      back_to_shop_text: "",
    }),
  },
});
</script>

<template>
  <!--  -->
  <div class="flex items-center justify-center">
    <!--  -->
    <div class="flex flex-col items-center justify-center gap-1">
      <!--  -->
      <div class="flex items-end justify-center">
        <!--  -->
        <Icon
          name="raphael:package"
          class="w-32 h-16 text-third md:w-40 md:h-16"
        />
        <!--  -->
      </div>
      <!--  -->

      <!--  -->
      <div
        v-if="texts.empty_text"
        class="flex flex-col items-center gap-3 md:gap-4"
      >
        <!--  -->
        <span class="font-light text-sm text-[#0f0f0f]">{{
          texts.empty_text
        }}</span>
        <!--  -->

        <!--  -->
        <!-- <NuxtLink
          v-if="buttons.shop_text"
          to="/shop"
          class="h-10 w-fit flex items-center justify-center px-8 cursor-pointer rounded-full border border-blue-500 bg-blue-500 text-white hover:bg-transparent hover:text-blue-500 transition-all duration-300"
        >

          <span class="text-sm font-medium">{{ buttons.shop_text }}</span>

        </NuxtLink> -->
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
