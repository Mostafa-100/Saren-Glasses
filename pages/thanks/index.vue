<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import { useMainStore } from "@/stores/main";

const { $settings, $storeino, $tools }: any = useNuxtApp();
const $store = useMainStore();
const route = useRoute();

const analyticsShare = ref($store.settings.analitycs_shere || []);
const thanks = $settings.sections.thanks;

const texts = ref(thanks.texts);
const buttons = ref(thanks.buttons);

onMounted(() => {
  $storeino.fbpx("PageView");
  $storeino.fbpx("Purchase");

  const win: any = window;

  if (analyticsShare.value?.[0]?.["data-website-id"] && win.umami) {
    const pixelData = route.query.pixel
      ? JSON.parse(route.query.pixel as string)
      : {};
    win.umami.track("Purchase", {
      price: pixelData.total,
      orderId: route.query.code,
      currency: $store.currency?.code || "",
      item_ids: pixelData.content_ids,
      click_id: route.query.ScCid || null,
    });
  }

  $tools.call("PAGE_VIEW");

  if (route.query.pixel) {
    const pixelData = JSON.parse(route.query.pixel as string);
    win.snapPurchase({
      price: pixelData.total,
      transaction_id: route.query.code,
      currency: $store.currency?.code || "",
      item_ids: pixelData.content_ids,
      click_id: route.query.ScCid || null,
    });
  }
});
</script>

<template>
  <div
    class="container flex items-center justify-center py-[112px] min-h-dvh bg-[#F1F1F1]"
  >
    <div class="w-full max-w-3xl mx-auto">
      <div
        class="bg-[#FAFAFA] rounded-2xl shadow-md overflow-hidden border border-[#E0E0E0]"
      >
        <!-- Top section -->
        <div
          class="flex flex-col gap-6 md:gap-8 bg-white p-6 md:p-8 border-b border-[#E0E0E0]"
        >
          <div class="flex justify-center">
            <div class="relative">
              <div
                class="absolute inset-0 animate-ping rounded-full bg-green-100"
              ></div>
              <div
                class="h-20 w-20 flex items-center justify-center rounded-full bg-green-100"
              >
                <Icon
                  name="ph:check-circle-bold"
                  class="w-12 h-12 text-green-500"
                />
              </div>
            </div>
          </div>

          <div class="flex flex-col items-center gap-3 animate-fade">
            <h1 class="text-2xl md:text-3xl font-bold text-black">
              {{ texts.title }}
            </h1>
            <p class="text-lg text-gray-700">{{ texts.description }}</p>
          </div>
        </div>

        <!-- Bottom section -->
        <div class="flex flex-col gap-6 md:gap-8 p-6 md:p-8">
          <div v-if="route.query.code" class="flex justify-center animate-fade">
            <div
              class="w-fit h-12 flex gap-2 md:gap-4 items-center justify-center px-4 md:px-8 rounded-lg border border-gray-400 bg-white text-gray-800"
            >
              <Icon name="ph:receipt" class="text-2xl" />
              <span class="text-base font-medium">{{
                `${buttons.order_text} ${route.query.code}`
              }}</span>
            </div>
          </div>

          <div
            class="flex flex-col sm:flex-row justify-center items-center gap-6 md:gap-8 animate-fade"
          >
            <NuxtLink
              to="/shop"
              class="w-full h-12 flex gap-2 md:gap-4 items-center justify-center px-4 md:px-8 cursor-pointer rounded-full border border-gray-400 text-gray-800 hover:bg-gray-200 transition-colors"
            >
              <Icon name="solar:cart-3-linear" class="text-2xl" />
              <span class="text-base font-medium">{{ buttons.shop_text }}</span>
            </NuxtLink>

            <NuxtLink
              to="/account/orders"
              class="w-full h-12 flex gap-2 md:gap-4 items-center justify-center px-4 md:px-8 cursor-pointer rounded-full border border-black text-black hover:bg-black hover:text-white transition-colors"
            >
              <Icon name="solar:box-linear" class="text-2xl" />
              <span class="text-base font-medium">{{
                buttons.track_text
              }}</span>
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes fade {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade {
  animation: fade 0.6s ease-out;
}
</style>
