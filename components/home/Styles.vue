<script setup>
const { $settings } = useNuxtApp();
const { styles } = $settings.sections;

const products = chunkArray(styles.products, 3);

function getDirection(index) {
  return index % 2 === 0 ? "md:self-end" : "md:self-start";
}

function chunkArray(arr, size) {
  const result = [];
  for (let i = 0; i < arr.length; i += size) {
    result.push(arr.slice(i, i + size));
  }
  return result;
}
</script>

<template>
  <div class="py-8 bg-[#f1f1f1]">
    <div class="container mx-auto">
      <div class="mb-6 md:w-[400px]">
        <GlobalSectionHeader :content="styles.content.header" />
        <p class="text-sm">{{ styles.content.description }}</p>
      </div>
      <div class="flex flex-col">
        <div
          v-for="(threeProducts, index) in products"
          :class="[
            'flex gap-4 flex-col flex-wrap justify-center md:flex-row mt-4',
            getDirection(index),
          ]"
          :key="index"
        >
          <div
            :key="index"
            v-for="(product, index) in threeProducts"
            class="md:w-[285px]"
          >
            <img :src="product.images[0].src" />
            <h3 class="text-[14px] font-[600] uppercase">{{ product.name }}</h3>
            <p class="text-sm my-2">
              {{
                product.description.slice(0, 118) +
                (product.description.length > 118 ? "..." : "")
              }}
            </p>
            <GlobalSectionLink
              label="Browse"
              :link="`/shop/${product.collections[0]?.slug ?? ''}`"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
