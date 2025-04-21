<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useMainStore } from "@/stores/main";

const props: any = defineProps({
  page: { type: String, require: false, default: "home" },
  upsell: { type: Object, default: null },
  item: Object,
  textColorClass: {
    type: String,
    default: "",
  },
  imageSizeClasses: {
    type: String,
    default: "",
  },
});

const { $settings, $storeino, $tools }: any = useNuxtApp();
const $store = useMainStore();

const products = $settings.sections.products;
const alerts = ref($settings.sections.alerts);

const inStockColor = ref(products.stock.in_stock_color);
const outOfStockColor = ref(products.stock.out_of_stock_color);

const reviewsInActiveColor = ref(products.reviews.inactive_color);
const reviewsActiveColor = ref(products.reviews.active_color);

const sizes = ref(props.item.sizes);
const brand = ref(props.item.brand);

const tags = ref(props.item.tags);
const collections = ref(props.item.collections);

const reference = ref(props.item.reference);

const variant = ref(
  props.item.type === "variant" ? props.item.variants?.[0] : null
);

const quantity = ref(props.item.quantity);

const discount = ref(props.upsell ? props.upsell.discount : null);
const outOfStock = ref(false);

const isInWishlist = computed(() =>
  $store.wishlist.some((item) => item._id === props.item._id)
);

const stockText = computed(() =>
  outOfStock.value
    ? products.stock.out_of_stock_text
    : products.stock.in_stock_ext
);

const stockColor = computed(() =>
  outOfStock.value ? outOfStockColor.value : inStockColor.value
);

onMounted(() => {
  if (props.item.type === "simple") {
    handleSimpleProduct();
  } else {
    handleVariantProduct();
  }
});

const handleSimpleProduct = () => {
  if (discount.value) {
    props.item.originalPrice = $tools.copy(props.item.price);
    if (discount.value.type === "percentage") {
      props.item.price.salePrice =
        props.item.price.salePrice -
        (props.item.price.salePrice * discount.value.value) / 100;
      props.item.price.comparePrice =
        props.item.price.comparePrice -
        (props.item.price.comparePrice * discount.value.value) / 100;
    } else {
      props.item.price.salePrice =
        props.item.price.salePrice - discount.value.value;
      props.item.price.comparePrice =
        props.item.price.comparePrice - discount.value.value;
    }
  }
  checkOutOfStock();
};

const handleVariantProduct = () => {
  if (discount.value) {
    props.item.variants?.forEach((variant: any) => {
      variant.originalPrice = $tools.copy(variant.price);
      if (discount.value?.type === "percentage") {
        variant.price.salePrice =
          variant.price.salePrice -
          (variant.price.salePrice * discount.value.value) / 100;
        variant.price.comparePrice =
          variant.price.comparePrice -
          (variant.price.comparePrice * discount.value.value) / 100;
      } else {
        variant.price.salePrice =
          variant.price.salePrice - discount.value!.value;
        variant.price.comparePrice =
          variant.price.comparePrice - discount.value!.value;
      }
    });
  }
  checkOutOfStock();
};

const checkOutOfStock = () => {
  if (props.item.type === "simple") {
    outOfStock.value =
      !props.item.outStock.disabled && props.item.quantity.instock <= 0;
  } else {
    outOfStock.value =
      !props.item.outStock.disabled &&
      props.item.variants?.[0] &&
      props.item.variants[0].quantity.instock <= 0;
  }
};

const addToCart = () => {
  const item = {
    _id: props.item._id,
    quantity: quantity.value.value || props.item.quantity.default,
    price: variant.value
      ? variant.value.price.salePrice
      : props.item.price.salePrice,
    variant: variant.value ? { _id: variant.value._id } : null,
    upsell: props.upsell,
  };

  $tools.call("ADD_TO_CART", item);
  $tools.toast(alerts.value.cart.added_text);

  if (products.add_to_cart.to_checkout) {
    setTimeout(() => {
      window.location.href = "/checkout2";
    }, 500);
  }

  $storeino.fbpx("AddToCart", {
    content_name: props.item.name,
    content_ids: [props.item._id],
    content_type: "product",
    value: variant.value
      ? variant.value.price.salePrice
      : props.item.price.salePrice,
    currency: $store.currency?.code || "USD",
  });
};

const toggleWishlist = () => {
  if (isInWishlist.value) {
    removeFromWishlist();
  } else {
    addToWishlist();
  }
};

const addToWishlist = () => {
  $tools.call("ADD_TO_WISHLIST", props.item);
  $tools.toast(alerts.value.wishlist.added_text);
};

const removeFromWishlist = () => {
  $tools.call("REMOVE_FROM_WISHLIST", props.item);
  $tools.toast(alerts.value.wishlist.removed_text);
};
</script>

<template>
  <div class="relative h-64 overflow-hidden">
    <!-- Background image -->
    <img
      :src="item.images[0].src"
      alt="Product image"
      class="absolute inset-0 w-full h-full object-cover"
    />

    <!-- Foreground content -->
    <div class="relative z-10 flex justify-between items-end p-4 h-full">
      <!-- Left section: title + price -->
      <div class="flex flex-col justify-end max-w-[80%]">
        <NuxtLink
          :to="`/products/${item.slug}`"
          class="text-sm uppercase text-[#0F0F0F] font-medium"
        >
          {{ item.name }}
        </NuxtLink>

        <ProductPrice
          page="home"
          :type="item.type"
          :price="item.price"
          :variants="item.variants"
          price-style="text-sm font-light text-[#0F0F0F]"
          sub-price-style="text-sm font-normal text-[#0F0F0F]"
        />
      </div>

      <!-- Right section: wishlist button -->
      <div class="flex gap-x-1">
        <div v-if="products.add_to_wishlist.active" class="wishlist-icon">
          <!--  -->
          <button
            :title="isInWishlist ? 'Remove from Wishlist' : 'Add to Wishlist'"
            :class="`h-9 w-9 flex items-center justify-center text-[#0F0F0F]`"
            @click="toggleWishlist"
          >
            <!--  -->
            <Icon
              :name="isInWishlist ? 'solar:heart-bold' : 'solar:heart-linear'"
              class="flex items-center justify-center text-lg text-[#0F0F0F]"
            />
            <!--  -->
          </button>
          <!--  -->
        </div>
        <div
          v-if="products.add_to_cart.active"
          class="flex flex-col items-end justify-center"
        >
          <!--  -->
          <button
            v-if="
              !outOfStock &&
              props.item.type == 'simple' &&
              props.item.price.salePrice > 0
            "
            class="h-9 w-9 flex items-center justify-center text-[#0F0F0F] hover:text-secondary transition-all duration-300"
            @click="addToCart()"
          >
            <!--  -->
            <Icon
              name="solar:cart-large-2-linear"
              :class="`text-lg translate text-[#0F0F0F]`"
            />
            <!--  -->
          </button>
          <!--  -->

          <!--  -->
          <!--  -->
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wishlist:hover .wishlist-icon {
  opacity: 1;
}

.wishlist .wishlist-icon > button {
  transition: all 0.3s ease-in-out;
}

.wishlist:hover .wishlist-icon > button {
  transform: translateY(-0.5rem);
}
</style>
