<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import { useMainStore } from "@/stores/main";

const { $storeino, $settings }: any = useNuxtApp();
const $store = useMainStore();

const items: any = ref([]);
const loading = ref(true);
let collections = $settings.sections.collections;

console.log("Collections", collections);

// collections = splitArray(collections);

// TODO: Splite collections into two parts one contain 3 collections, and rest contain the rest

function splitArray(arr) {
  const firstThree = arr.slice(0, 3);
  const rest = arr.slice(3);
  return [firstThree, rest];
}

const swiperBreakpoints = {
  320: { slidesPerView: 2.5 },
  640: { slidesPerView: 3.5 },
  768: { slidesPerView: 4.5 },
  960: { slidesPerView: 5.5 },
  1080: { slidesPerView: 6.5 },
  1280: { slidesPerView: 7.5 },
};

const navigation = ref({
  nextEl: ".swiper-button-next",
  prevEl: ".swiper-button-prev",
});

const languageCode = computed(() => $store.language.code);

const noProductsMessage = computed(() => {
  switch (languageCode.value) {
    case "AR":
      return "ليس لديك أي مجموعات";
    case "FR":
      return "Vous n'avez aucune collection";
    default:
      return "You don't have any collection";
  }
});

const goToAdminMessage = computed(() => {
  switch (languageCode.value) {
    case "AR":
      return "انتقل إلى مسؤول المتجر الخاص بك > المنتجات > المجموعات";
    case "FR":
      return "Accédez à l'administration de votre boutique > produits > collections >";
    default:
      return "Go to your store admin > products > collections >";
  }
});

const addNewLabel = computed(() => {
  switch (languageCode.value) {
    case "AR":
      return "اضف جديد";
    case "FR":
      return "Ajouter nouveau";
    default:
      return "Add new";
  }
});

const viewDemoLabel = computed(() => {
  switch (languageCode.value) {
    case "AR":
      return "شاهد العرض التوضيحي للسمة";
    case "FR":
      return "Voir la démo du thème";
    default:
      return "View theme demo";
  }
});

const fetchCollections = async () => {
  try {
    const filter = {};
    if (collections.items.length > 0) {
      items.value = splitArray(collections.items);
    } else {
      const { data } = await $storeino.collections.search(filter);
      items.value = splitArray(data.results);
    }
  } catch (err) {
    console.error({ err });
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchCollections();
});
</script>

<template>
  <!--  -->
  <div class="container mt-8 md:mt-12 lg:mt-16">
    <!--  -->
    <div v-if="loading" class="flex items-center justify-center my-10">
      <!--  -->
      <LoaderGlobal></LoaderGlobal>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div v-if="!loading && items.length > 0" class="flex flex-col gap-6">
      <!--  -->
      <GlobalSectionHeader
        v-if="collections.title.length > 0"
        :content="collections.title"
      />
      <!--  -->
      <ClientOnly>
        <!--  -->
        <div class="flex flex-col md:flex-row gap-3">
          <ElementCollection
            v-for="(item, index) in items[0]"
            :key="index"
            :item="item"
            collectionHeightClass="h-[550px]"
          />
        </div>

        <div class="flex flex-col gap-3">
          <ElementCollection
            v-for="(item, index) in items[1]"
            :key="index"
            :item="item"
            collectionHeightClass="h-[460px]"
          />
        </div>
        <!--  -->
        <!--  -->
      </ClientOnly>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <div v-if="!loading && items.length === 0" class="my-10">
      <!--  -->
      <div
        class="p-5 flex flex-col lg:flex-row gap-6 lg:gap-0 items-center border-2 border-primary rounded-lg"
      >
        <!--  -->
        <div class="flex items-center justify-center w-full lg:w-1/2">
          <!--  -->
          <Icon
            name="solar:notification-unread-linear"
            class="flex items-center justify-center text-9xl text-primary translate"
          />
          <!--  -->
        </div>
        <!--  -->

        <!--  -->
        <div class="flex flex-col gap-2">
          <!--  -->
          <h2
            class="title-font text-lg lg:text-xl font-extrabold text-center lg:text-start"
          >
            {{ noProductsMessage }}
          </h2>
          <!--  -->

          <!--  -->
          <p class="text-sm font-medium lg:text-center">
            {{ goToAdminMessage }}
            <!--  -->
            <a
              href="//business.storeino.com/stores/products"
              target="_blank"
              class="underline text-primary font-bold hover:text-secondary"
            >
              {{ addNewLabel }}
            </a>
            <!--  -->
          </p>
          <!--  -->

          <!--  -->
          <div class="flex justify-center lg:justify-start">
            <!--  -->
            <a
              href="https://marketmall-theme.storeino.com"
              target="_blank"
              class="h-10 px-8 flex items-center justify-center font-medium rounded-full bg-secondary text-white border border-secondary hover:bg-transparent hover:text-secondary transition-all duration-300"
            >
              <!--  -->
              <span class="text-sm font-normal">{{ viewDemoLabel }}</span>
              <!--  -->
            </a>
            <!--  -->
          </div>
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
