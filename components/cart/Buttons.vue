<script setup lang="ts">
const props: any = defineProps({
  buttons: Object,
});
</script>

<template>
  <!--  -->
  <div class="flex flex-col gap-4">
    <!--  -->
    <a
      href="/checkout2"
      class="w-full h-10 flex items-center justify-center cursor-pointer rounded-md border border-[#d1d1d1] bg-transparent text-white click-effect"
    >
      <!--  -->
      <span class="text-sm font-light">
        {{ props.buttons.order_text }}
      </span>
      <!--  -->
    </a>
    <!--  -->

    <!--  -->
    <NuxtLink
      v-if="props.buttons.show_shop"
      to="/shop"
      class="w-full h-10 flex items-center justify-center cursor-pointer rounded-md border border-[#d1d1d1] text-white bg-transparent"
    >
      <!--  -->
      <span class="text-sm font-light">
        {{ props.buttons.shop_text }}
      </span>
      <!--  -->
    </NuxtLink>
    <!--  -->
  </div>
  <!--  -->
</template>
