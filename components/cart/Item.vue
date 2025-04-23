<script setup lang="ts">
import { computed, watch } from "vue";
import { useMainStore } from "@/stores/main";

const { $tools }: any = useNuxtApp();
const $store = useMainStore();

const props = defineProps({
  item: { type: Object, required: true },
  quantity: { type: Number, default: 1 },
});

const emit = defineEmits(["remove"]);

const formattedPrice = computed(() => {
  return (price: any) =>
    price % 1 !== 0 ? price.toFixed(2) : price.toString();
});

const quantitySelected = (quantity: any) => {
  props.item.quantity.value = quantity;
  props.item.total = props.item.price * quantity;

  let cartItem = null;
  if (props.item.variant) {
    cartItem = $store.cart.find(
      (i: any) =>
        i._id === props.item._id &&
        props.item.value &&
        i.variant._id === props.item.variant._id
    );
  } else {
    cartItem = $store.cart.find(
      (i) => i._id === props.item._id && props.item.value && !i.variant
    );
  }

  $tools.call("ADD_TO_CART", { ...cartItem, quantity });
};

watch(
  () => props.quantity,
  (newQuantity) => {
    quantitySelected(newQuantity);
  },
  { immediate: true }
);

const remove = () => {
  emit("remove", props.item);
};

// Temp
console.log("Cart item", props.item);
</script>

<template>
  <!--  -->
  <div
    class="relative flex flex-col md:flex-row items-center gap-1 md:gap-2 lg:gap-4 rounded-lg border border-third bg-white p-3"
  >
    <!--  -->
    <div class="w-full flex-col sm:flex-row flex gap-2">
      <!--  -->
      <NuxtLink
        :to="`/products/${props.item.slug}`"
        :title="props.item.name"
        :aria-label="props.item.name"
      >
        <!--  -->
        <ImageLoader
          img-class="w-full md:max-w-[180px]"
          :src="props.item.image"
          :alt="props.item.name"
        />
        <!--  -->
      </NuxtLink>
      <!--  -->

      <!--  -->
      <div class="w-4/6 flex flex-col gap-2">
        <!--  -->
        <NuxtLink
          :to="`/products/${props.item.slug}`"
          class="uppercase text-[18px] font-[600]"
        >
          {{ props.item.name }}
        </NuxtLink>
        <!--  -->

        <!--  -->
        <div
          v-if="props.item.variant"
          class="text-xs text-third text-[#8E9194]"
        >
          <!--  -->
          {{ props.item.variant.name }}
          <!--  -->
        </div>
        <!--  -->
        <div class="text-sm">
          {{ props.item.quantity.value }} x {{ props.item.price }}
          {{ $store.currency.symbol }}
        </div>
        <!--  -->
        <div class="w-fit">
          <ProductQuantity
            :quantity="props.item.quantity"
            @selected="quantitySelected"
          />
        </div>
        <!--  -->
        <div
          v-if="props.item.upsell"
          class="w-fit h-5 flex items-center justify-center px-2 rounded-sm bg-primary text-white"
        >
          <!--  -->
          <span class="text-xs font-normal"
            >-{{
              `${props.item.upsell.value} ${
                props.item.upsell.type == "percentage"
                  ? "%"
                  : $store.currency.symbol
              }`
            }}</span
          >
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div
      class="w-full md:w-1/3 flex flex-row md:flex-col items-center md:items-end justify-between md:justify-center gap-2"
    >
      <!--  -->
      <div class="absolute top-2 right-2 cursor-pointer" @click="remove">
        <!--  -->
        <Icon name="system-uicons:close" class="text-[29px]" />
        <!--  -->
      </div>
      <!--  -->

      <!--  -->
      <!--  -->

      <!--  -->
      <div
        class="flex flex-col items-end justify-center absolute bottom-3 right-3"
      >
        <!--  -->
        <span class="text-sm">
          {{ `${formattedPrice(props.item.total)} ${$store.currency.symbol}` }}
        </span>
        <!--  -->

        <!--  -->
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
