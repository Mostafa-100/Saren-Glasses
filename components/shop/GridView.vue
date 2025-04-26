<script setup lang="ts">
import fourColumnsIcon from "@/assets/four-columns.png";
import threeColumnsIcon from "@/assets/three-columns.png";
import twoColumnsIcon from "@/assets/two-columns.png";

defineProps({
  modelValue: {
    type: String,
    required: true,
    validator: (value: any) =>
      [
        "w-full",
        "w-full md:w-1/2",
        "w-1/2 md:w-1/3",
        "w-1/2 md:w-1/3 lg:w-1/4",
      ].includes(value),
  },
});

defineEmits(["update:modelValue"]);

const gridOptions = [
  {
    value: "w-full sm:w-1/2 md:w-1/2",
    icon: "circum:grid-2-v",
    label: twoColumnsIcon,
  },
  {
    value: "w-full sm:w-1/2 md:w-1/3",
    icon: "circum:grid-3-1",
    label: threeColumnsIcon,
  },
  {
    value: "w-full sm:w-1/2 md:w-1/3 lg:w-1/4",
    icon: "circum:grid-4-2",
    label: fourColumnsIcon,
  },
];
</script>

<template>
  <!--  -->
  <div class="w-fit md:min-w-48 h-10 hidden md:flex gap-2 items-center px-4">
    <!--  -->
    <button
      v-for="option in gridOptions"
      :key="option.value"
      type="button"
      class="flex items-center justify-center cursor-pointer rounded-lg bg-third"
      @click="$emit('update:modelValue', option.value)"
    >
      <!--  -->
      <img
        :src="option.label"
        :alt="option.value"
        class="w-6 h-6"
        :class="{ 'brightness-[9]': modelValue === option.value }"
      />
      <!--  -->
    </button>
    <!--  -->
  </div>
  <!--  -->
</template>
