<script setup>
import { ref, watch } from "vue";
import { useRouter, useRoute } from "#app";
import { useMainStore } from "@/stores/main";

const props = defineProps({
  isActive: {
    type: Boolean,
    required: true,
  },
  action: {
    type: String,
    default: "/shop",
  },
  placeholder: {
    type: String,
    default: "Search...",
  },
  searchBgColor: {
    type: String,
    default: "#f0f0f0",
  },
  searchTextColor: {
    type: String,
    default: "#000",
  },
});

const $store = useMainStore();

const router = useRouter();
const route = useRoute();

const searchQuery = ref(route.query.search || $store.search);

watch(searchQuery, (newValue) => {
  $store.setSearch(newValue);
});

const goSearch = () => {
  router.push({ path: "/shop", query: { search: searchQuery.value } });
};
</script>

<template>
  <form
    v-if="props.isActive"
    class="w-full flex items-center justify-between px-4 lg:px-5 search-text border-b border-black"
    :action="props.action"
    :style="{
      backgroundColor: props.searchBgColor,
      color: props.searchTextColor,
    }"
    @submit.prevent="goSearch"
  >
    <input
      v-model="searchQuery"
      class="w-full text-sm font-medium focus:outline-none bg-inherit placeholder:text-inherit placeholder:font-medium grow"
      :placeholder="props.placeholder"
      type="search"
      name="searchQuery"
    />
    <button class="flex items-center justify-center" aria-label="Search button">
      <Icon name="solar:magnifer-linear" class="text-xl translate mb-1" />
    </button>
  </form>
</template>
