<script setup lang="ts">
import { useMainStore } from "@/stores/main";

const $store = useMainStore();

const props = defineProps({
  banner: {
    type: Object,
    required: true,
  },
  type: {
    type: String as () => "collection" | "default",
    required: true,
    validator: (value: string) => ["collection", "default"].includes(value),
  },
});

// Templ
console.log("Banner", props.banner);

const isVisible = computed(() => {
  if (props.type === "collection") return true;
  return props.banner.active;
});

const bannerName: any = computed(() => {
  if (props.type === "collection") {
    return props.banner?.name;
  }
  return props.banner.title;
});

const shopImage: any = computed(() => {
  if (props.type === "collection") {
    return props.banner?.image?.src;
  }
  return props.banner?.image?.src || $store.defaults.shopImage;
});
</script>

<template>
  <!--  -->
  <div v-if="isVisible" class="w-full py-5 px-4 mb-5">
    <!--  -->
    <!-- <ImageLoader
      img-class="object-cover w-full h-full"
      :src="shopImage"
      alt="banner image"
    /> -->
    <!--  -->

    <!--  -->
    <div class="max-w-[300px]">
      <!--  -->
      <h1 class="text-xl lg:text-2xl uppercase">
        {{ bannerName }}
      </h1>
      <p v-if="props.banner.description" class="text-sm mt-3">
        {{ props.banner.description }}
      </p>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
