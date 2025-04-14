<script setup lang="ts">
import { ref } from "vue";
import { useNuxtApp } from "#app";

const props = defineProps({
  textColor: {
    type: String,
    required: true,
  },
  bgColor: {
    type: String,
    required: true,
  },
});

const { $settings }: any = useNuxtApp();

const header = $settings.sections.header;
const headerMenu = ref(header.menu);
const collectionsMenu = ref(header.collections.menu);
const headerRightMenu = ref(header.rightMenu);
</script>

<template>
  <div
    v-if="collectionsMenu || headerMenu || headerRightMenu"
    class="relative hidden lg:block"
  >
    <!--  -->
    <div class="container flex items-center justify-between py-2">
      <!--  -->
      <MenusPages
        :header-menu="headerMenu"
        :bg-color="props.bgColor"
        :text-color="props.textColor"
        class="w-2/4"
      />

      <MenusPages
        :header-menu="headerRightMenu"
        :bg-color="props.bgColor"
        :text-color="props.textColor"
        class="w-2/4 justify-end"
      />
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>

<style scoped>
[dir="rtl"] .mr-16-ar {
  margin-left: 4rem;
  margin-right: 0;
}

::placeholder {
  color: var(--search-text-color);
}

[dir="rtl"] .col-width-ar {
  right: 100%;
  left: 0px;
}

[dir="rtl"] .cart-direction {
  left: 0px;
  right: auto;
}

.col-width {
  width: 14.5rem;
}

@media (min-width: 1120px) {
  .col-width {
    width: 15rem;
  }
}

@media (min-width: 1150px) {
  .col-width {
    width: 16rem;
  }
}

@media (min-width: 1200px) {
  .col-width {
    width: 16.5rem;
  }
}
</style>
