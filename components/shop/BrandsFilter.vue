<script setup lang="ts">
const props: any = defineProps({
  sidebar: {
    type: Object,
    required: true,
  },
  brands: {
    type: Array,
    required: true,
  },
  params: {
    type: Object,
    required: true,
  },
  setParams: {
    type: Function,
    required: true,
  },
});

const isOpen = ref(false);

const toggleBrands = () => {
  isOpen.value = !isOpen.value;
};
</script>

<template>
  <!--  -->
  <div
    v-if="props.sidebar.brands.active && props.brands.length > 0"
    class="overflow-hidden"
  >
    <!--  -->
    <div
      v-if="sidebar.brands.title.length > 0"
      class="h-10 flex items-center justify-between bg-third px-4 cursor-pointer"
    >
      <!--  -->
      <h2 class="text-sm capitalize">
        {{ sidebar.brands.title }}
      </h2>
      <!--  -->

      <!--  -->
      <!-- <Icon
        :name="'solar:alt-arrow-down-linear'"
        :class="[
          'text-xl transition-transform duration-300',
          { 'rotate-180': isOpen },
        ]"
      /> -->
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <transition name="slide-down">
      <!--  -->
      <div class="flex flex-col gap-3 px-4 py-3">
        <!--  -->
        <div v-for="(item, i) in props.brands" :key="i">
          <!--  -->
          <label class="relative flex items-center w-full gap-4 cursor-pointer">
            <!--  -->
            <input
              :id="item.slug"
              hidden
              type="checkbox"
              class="absolute top-0 left-0 form-checkbox"
              style="z-index: -1"
              :checked="
                props.params['brand.name-in'] &&
                props.params['brand.name-in'].indexOf(item.name) >= 0
              "
              @change="props.setParams($event, 'brand.name-in', item.name)"
            />
            <!--  -->
            <div class="w-4 h-4 flex justify-center items-center bg-white">
              <!--  -->
              <!-- <Icon
                name="material-symbols:check-rounded"
                class="hidden text-xl translate text-white"
              /> -->
              <!--  -->
            </div>
            <!--  -->

            <!--  -->
            <label
              class="capitalize cursor-pointer text-sm text-gray"
              :for="item.name"
            >
              {{ item.name }}
            </label>
            <!--  -->
          </label>
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
