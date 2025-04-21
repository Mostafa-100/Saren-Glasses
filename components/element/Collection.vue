<script setup lang="ts">
const props: any = defineProps({
  item: {
    type: Object,
    required: true,
  },
  page: {
    type: String,
    required: false,
    default: "",
  },
  collectionClass: {
    type: String,
    default: "",
  },
});

const { $settings }: any = useNuxtApp();
const collections = $settings.sections.collections;
</script>

<template>
  <!--  -->
  <div
    :class="`relative grid place-items-center grow ${props.collectionClass}`"
  >
    <img
      :src="props.item.image ? props.item.image.src : ''"
      class="absolute left-0 top-0 w-full h-full brightness-50 -z-10 object-cover"
    />
    <div class="text-center">
      <div class="text-white uppercase text-lg font-[600]">
        {{ props.item.name }}
      </div>
      <GlobalSectionLink
        label="View Products"
        :link="
          props.item.childrens.length > 0
            ? `/collections/${props.item.slug}`
            : `/shop/${props.item.slug}`
        "
        otherClass="text-white justify-center mt-4"
      />
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>

<style scoped>
@keyframes subtle-bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-4px);
  }
}

.group:hover img {
  animation: subtle-bounce 1s ease infinite;
}
</style>
