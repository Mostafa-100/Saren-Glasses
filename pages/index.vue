<script setup lang="ts">
import { computed } from "vue";
import { useNuxtApp } from "#app";
import { useMainStore } from "@/stores/main";
import Selections from "~/components/home/Selections.vue";

const $store = useMainStore();

const { $settings, $storeino, $tools }: any = useNuxtApp();
const {
  carousel,
  collections,
  products,
  offers,
  hero,
  styles,
  selections,
  location,
} = $settings.sections;

const productsItems = computed(() => {
  return products?.items ? Object.values(products.items) : [];
});

$storeino.fbpx("PageView");

onMounted(() => {
  // SEO
  $store.seo.title = $settings.store_name;
  $store.seo.description = $settings.store_description;

  // Pixels
  $tools.call("PAGE_VIEW");
  $storeino.fbpx("PageView");
});
</script>

<template>
  <div class="home-page">
    <!-- Carousel -->
    <!-- <HomeSlider v-if="carousel.active" /> -->
    <HomeHero v-if="hero.active" />

    <!-- Offers -->
    <!-- <HomeOffers v-if="offers.active" /> -->

    <!-- Styles -->
    <HomeStyles v-if="styles.active" />
    <HomeSelections v-if="selections.active" />

    <!-- Home Events -->
    <!-- <HomeEvents />   -->

    <!-- Products  -->
    <template v-if="products.active">
      <HomeProducts
        v-for="(product, index) in productsItems"
        v-if="productsItems.length > 0"
        :key="index"
        :product="product"
      />
    </template>

    <!-- Collections -->
    <HomeCollections v-if="collections.active" />

    <!-- Location -->
    <HomeLocation v-if="location.active" />

    <!-- Services -->
    <HomeServices />

    <!--  -->
    <div class="container">
      <AppsLoader placement="BEFORE_HOME_FOOTER" />
    </div>
    <!--  -->
  </div>
</template>
