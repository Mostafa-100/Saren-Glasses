<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
  sortLabels: {
    type: Object,
    required: true,
  },
  params: {
    type: Object,
    required: true,
  },
});

const windowWidth = ref<number | null>(null);
const isSortVisible = ref(false);

const updateWindowWidth = () => {
  if (typeof window !== "undefined") {
    windowWidth.value = window.innerWidth;
  }
};

onMounted(() => {
  if (typeof window !== "undefined") {
    windowWidth.value = window.innerWidth;
    window.addEventListener("resize", updateWindowWidth);
  }
});

onUnmounted(() => {
  if (typeof window !== "undefined") {
    window.removeEventListener("resize", updateWindowWidth);
  }
});

const toggleSortVisibility = (force: boolean | null) => {
  if (windowWidth.value !== null && windowWidth.value >= 1024) {
    isSortVisible.value = force !== null ? force : isSortVisible.value;
  } else if (force === null) {
    isSortVisible.value = !isSortVisible.value;
  }
};

const sortOptions = [
  ["price.salePrice", 1, "price_asc"],
  ["price.salePrice", -1, "price_desc"],
  ["review.rating", -1, "rating_desc"],
  ["review.rating", 1, "rating_asc"],
  ["name", 1, "name_asc"],
  ["name", -1, "name_desc"],
  ["createdAt", -1, "newest"],
  ["createdAt", 1, "oldest"],
];

const sorts = sortOptions.map(([field, order, label]) => ({
  field: { [field]: order },
  name: props.sortLabels[label],
}));

const currentSortName = computed(() => {
  const currentSort = sorts.find(
    (sort) => JSON.stringify(sort.field) === JSON.stringify(props.params.sort)
  );
  return currentSort ? currentSort.name : "";
});
</script>

<template>
  <!--  -->
  <div
    class="relative z-10 flex items-center justify-center cursor-pointer"
    @mouseover="toggleSortVisibility(true)"
    @mouseleave="toggleSortVisibility(false)"
  >
    <!--  -->
    <div
      class="w-fit md:min-w-48 h-10 flex items-center justify-between gap-2 rounded-lg bg-third px-4"
      @click="toggleSortVisibility(null)"
    >
      <!--  -->
      <h2 class="text-sm text-[#0f0f0f]">
        <!-- {{ sortLabels.sort_by_text }}: -->
        <span>{{ currentSortName }}</span>
      </h2>
      <!--  -->

      <!--  -->
      <Icon name="ic:baseline-plus" class="text-sm translate" />
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <transition>
      <!--  -->
      <div
        v-if="isSortVisible"
        class="w-48 pt-2 lg:pt-3 absolute top-full z-20"
      >
        <!--  -->
        <div
          class="flex flex-col gap-3 py-4 px-6 shadow-full bg-white border border-third"
        >
          <!--  -->
          <div
            v-for="(sort, index) in sorts"
            :key="index"
            class="flex items-center"
          >
            <!--  -->
            <input
              :id="sort.name"
              v-model="params.sort"
              hidden
              type="radio"
              :value="sort.field"
            />
            <!--  -->

            <!--  -->
            <label class="cursor-pointer text-sm" :for="sort.name">
              {{ sort.name }}
            </label>
            <!--  -->
          </div>
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->
    </transition>
    <!--  -->
  </div>
  <!--  -->
</template>
