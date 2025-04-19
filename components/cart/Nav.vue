<script setup lang="ts">
defineProps({
  cartTitle: {
    type: String,
    required: true,
  },
  itemsLength: {
    type: Number,
    required: true,
  },
  productText: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <!--  -->
  <div class="flex items-center gap-x-1">
    <!--  -->
    <span class="uppercase text-[16px] font-[600]">
      {{ cartTitle }}
    </span>
    <!--  -->

    <!--  -->
    <span class="text-sm text-[#8E9194]">
      {{ `(${itemsLength})` }}
    </span>
    <!--  -->
  </div>
  <!--  -->
</template>
