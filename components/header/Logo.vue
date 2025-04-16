<script setup lang="ts">
import { useNuxtApp } from "#app";

const { $settings } = useNuxtApp();

const storeName = $settings.sections.footer.about.store_name;

// Props for the LogoComponent
const props = defineProps({
  mobileLogo: {
    type: Object,
    default: () => ({ img: null }),
  },
  desktopLogo: {
    type: Object,
    default: () => ({ img: null }),
  },
  defaultLogo: {
    type: String,
    default: "",
  },
});

// Computed logo source
const logoSrc = computed(() => {
  return (
    (props.mobileLogo.img || props.desktopLogo.img)?.src || props.defaultLogo
  );
});
</script>

<template>
  <NuxtLink to="/">
    <h1 class="tracking-[20px] text-xl">{{ storeName }}</h1>
    <!-- <h1 class="tracking-[20px] text-xl">{{ storeName }}</h1> -->

    <!-- <imageLoader
      property="height"
      img-class="object-contain w-full h-8 lg:h-10"
      :src="logoSrc"
      alt="Store Logo"
    /> -->
  </NuxtLink>
</template>
