<script setup lang="ts">
const props: any = defineProps({
  item: {
    type: Object,
    required: true,
  },
  product: {
    type: Object,
    required: true,
  },
  products: {
    type: Object,
    required: true,
  },
  isInWishlist: {
    type: Boolean,
    default: false,
  },
  socialMedia: {
    type: Array,
    default: () => [],
  },
  reviewsActiveColor: {
    type: String,
    default: "#FFA41C",
  },
  reviewsInActiveColor: {
    type: String,
    default: "#E6E6E6",
  },
  price: {
    type: Object,
  },
  stockColor: {
    type: String,
    require: true,
  },
  stockText: {
    type: String,
    require: true,
  },
});

const emit = defineEmits(["toggle-wishlist"]);

const filteredSocialMedia = computed(() => {
  return props.socialMedia.filter(
    (s: any) => props.product.share_buttons[s.name]
  );
});
</script>

<template>
  <!--  -->
  <div class="flex flex-col gap-3">
    <!--  -->
    <meta itemprop="name" :content="item.name" />
    <!--  -->

    <!--  -->
    <div>
      <div class="flex justify-between">
        <!--  -->
        <h2 class="text-sm uppercase md:leading-8 font-[600]">
          {{ item.name }}
        </h2>
        <!--  -->
        <!--  -->
        <div
          v-if="products.add_to_wishlist.active"
          class="w-fit flex justify-center"
        >
          <!--  -->
          <button
            :title="isInWishlist ? 'Remove from Wishlist' : 'Add to Wishlist'"
            class=""
            @click="$emit('toggle-wishlist')"
          >
            <!--  -->
            <Icon
              :name="isInWishlist ? 'solar:heart-bold' : 'solar:heart-linear'"
              class="flex items-center justify-center text-sm"
            />
            <!--  -->
          </button>
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->
      <ProductPrice
        page="product"
        :type="'simple'"
        :price="price"
        :variants="[]"
        price-style="text-sm"
        currency-style="text-sm text-[#0f0f0f]"
        sub-price-style="text-sm font-light"
      />
    </div>
    <!--  -->
    <div
      v-if="props.products.stock.active"
      class="h-5 flex items-center justify-center text-white mt-3 w-fit"
      :style="{ backgroundColor: props.stockColor }"
    >
      <!--  -->
      <span class="px-2 text-xs font-normal">{{ props.stockText }}</span>
      <!--  -->
    </div>
    <!--  -->
    <div v-if="products.reviews.active" class="flex gap-1.5 items-center">
      <!--  -->
      <div class="flex items-center">
        <!--  -->
        <Icon
          v-for="star in 5"
          :key="star"
          :name="
            star <= item.review.rating
              ? 'material-symbols:star-rate-rounded'
              : 'material-symbols:star-outline-rounded'
          "
          :style="{
            color:
              star <= item.review.rating
                ? reviewsActiveColor
                : reviewsInActiveColor,
          }"
          class="flex items-center justify-center text-lg"
        />
      </div>
      <!--  -->

      <!--  -->
      <span class="text-sm">
        {{ `(${item.review.reviews.length} ${product.reviews.reviews_name})` }}
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div v-if="item.description" class="flex flex-col gap-2">
      <!--  -->
      <h3 class="text-base font-bold text-primary">
        {{ product.content.title }}
      </h3>
      <!--  -->

      <!--  -->
      <p class="text-sm text-[#0f0f0f] font-[300]">{{ item.description }}</p>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      v-if="product.reference.active && item.reference"
      class="flex items-center"
    >
      <!--  -->
      <span class="text-s font-medium capitalize">
        {{ product.reference.title }}
        <!--  -->
        <NuxtLink
          :to="`/shop/?ref=${item.reference}`"
          class="font-normal hover:text-secondary"
        >
          {{ item.reference }}
          <!--  -->
        </NuxtLink>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      v-if="product.collections.active && item.collections.length > 0"
      class="collections flex items-center"
    >
      <!--  -->
      <span class="text-s font-medium capitalize">
        <!--  -->
        {{ product.collections.title }}
        <!--  -->

        <!--  -->
        <NuxtLink
          v-for="(collection, index) in item.collections"
          :key="index"
          :to="`/shop/${collection.name}`"
          class="font-normal cursor-pointer break"
        >
          <!--  -->
          <span class="hover:text-secondary">{{ collection.name }}</span>
          <!--  -->

          <!--  -->
          <span class="comma"> | </span>
          <!--  -->
        </NuxtLink>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      v-if="product.tags.active && item.tags.length > 0"
      class="tags flex items-center"
    >
      <!--  -->
      <span class="text-s font-medium capitalize">
        <!--  -->
        {{ product.tags.title }}
        <!--  -->

        <!--  -->
        <NuxtLink
          v-for="(tag, index) in item.tags"
          :key="index"
          class="font-normal cursor-pointer break"
        >
          <!--  -->
          <span class="hover:text-secondary">{{ tag }}</span>
          <!--  -->

          <!--  -->
          <span class="comma"> | </span>
          <!--  -->
        </NuxtLink>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div v-if="item.brand && products.brand.active" class="flex items-center">
      <!--  -->
      <span class="text-s font-medium capitalize">
        <!--  -->
        {{ product.brands.title }}
        <!--  -->

        <!--  -->
        <NuxtLink
          :to="`/shop/?brands=${item.brand.slug}`"
          class="font-normal hover:text-secondary"
        >
          <!--  -->
          {{ item.brand.name }}
          <!--  -->
        </NuxtLink>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      v-if="item.sizes?.weight && products.weight.active"
      class="flex items-center"
    >
      <!--  -->
      <span class="text-s font-medium capitalize">
        <!--  -->
        {{ product.weight.title }}
        <!--  -->
        <span class="font-normal">
          <!--  -->
          {{ item.sizes.weight.value + item.sizes.weight.unit }}
          <!--  -->
        </span>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      v-if="item.sizes?.dimensions && products.dimensions.active"
      class="flex items-center"
    >
      <!--  -->
      <span class="text-s font-medium capitalize">
        <!--  -->
        {{ `${products.dimensions.text} : ` }}
        <!--  -->

        <!--  -->
        <span class="font-normal">
          {{
            `${product.dimensions.width_text} ${item.sizes.dimensions.width + item.sizes.dimensions.unit},
            ${product.dimensions.height_text} ${item.sizes.dimensions.height + item.sizes.dimensions.unit},
            ${product.dimensions.depth_text} ${item.sizes.dimensions.depth + item.sizes.dimensions.unit}`
          }}
        </span>
        <!--  -->
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div v-if="product.share_buttons.active" class="flex flex-col gap-4">
      <!--  -->
      <h3
        v-if="product.share_buttons.title.length > 0"
        class="text-base font-bold text-primary"
      >
        <!--  -->
        {{ product.share_buttons.title }}
        <!--  -->
      </h3>
      <!--  -->

      <!--  -->
      <div class="flex items-center gap-4">
        <!--  -->
        <a
          v-for="item in filteredSocialMedia"
          :key="item.name"
          :href="item.url"
          target="_blank"
          rel="noopener noreferrer"
          class="w-10 h-10 flex items-center justify-center p-2 rounded-full hover:opacity-90 hover:scale-105 transition-all duration-300"
          :style="{
            backgroundColor: item.color,
          }"
        >
          <!--  -->
          <Icon :name="item.image" :class="item.size" class="text-white" />
          <!--  -->
        </a>
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>

<style scoped>
.collections .break:last-child .comma,
.tags .break:last-child .comma {
  display: none;
}
</style>
