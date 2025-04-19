<script setup lang="ts">
const props: any = defineProps({
  items: Object,
  total: Number,
  buttons: Object,
  texts: Object,
  currency: Object,
});

const formattedPrice = (price: number) => {
  return price % 1 !== 0 ? price.toFixed(2) : price.toString();
};
</script>

<template>
  <!--  -->
  <div class="flex flex-col rounded-lg py-4 px-6">
    <!--  -->
    <!--  -->

    <!--  -->
    <div class="flex items-center justify-between py-3">
      <!--  -->
      <span class="text-sm font-light">
        {{ props.texts.total_product_text }}
      </span>
      <!--  -->

      <!--  -->
      <span class="text-sm font-light">
        {{ `${props.items.length} ${props.texts.product_text}` }}
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div class="flex items-center justify-between py-3">
      <!--  -->
      <span class="text-sm font-light">
        {{ props.texts.subtotal_text }}
      </span>
      <!--  -->

      <!--  -->
      <span class="text-sm font-light">
        {{ `${formattedPrice(props.total)} ${props.currency.symbol}` }}
      </span>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div class="flex items-center justify-between pt-3">
      <!--  -->
      <span class="text-lg font-light">
        {{ props.texts.total_text }}
      </span>
      <!--  -->

      <!--  -->
      <span class="text-xl font-light">
        {{ `${formattedPrice(props.total)} ${props.currency.symbol}` }}
      </span>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
