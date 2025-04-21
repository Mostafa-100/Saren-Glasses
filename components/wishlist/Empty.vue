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
      <div class="flex justify-center">
        <!--  -->
        <Icon
          name="pepicons-pencil:heart-circle-off"
          class="w-12 h-12 text-third"
        />
        <!--  -->
      </div>
      <!--  -->

      <!--  -->
      <div v-if="texts.empty_text">
        <!--  -->
        <span class="font-light text-sm text-[#0f0f0f]">{{
          texts.empty_text
        }}</span>
        <!--  -->

        <!--  -->
        <!-- <NuxtLink
          v-if="buttons.shop_text"
          to="/"
          class="h-10 w-fit flex items-center justify-center px-8 cursor-pointer rounded-full border border-blue-500 bg-blue-500 text-white hover:bg-transparent hover:text-blue-500 transition-all duration-300"
        >

          <span class="text-sm">{{ buttons.shop_text }}</span>

        </NuxtLink> -->
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
