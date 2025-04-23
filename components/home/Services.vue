<script setup lang="ts">
import { computed } from "vue";
import { useNuxtApp } from "#app";
import { useMainStore } from "@/stores/main";

const { $settings }: any = useNuxtApp();

const $store = useMainStore();

const services = $settings.sections.services;

const bgColor = ref(services.colors.background_color);
const textColor = ref(services.colors.text_color);

const title = ref(services.content.title);
const description = ref(services.content.description);

const hasTitleOrDescription = computed(() => title.value || description.value);

const itemsArray: any = computed(() => Object.values(services.items || {}));
</script>

<template>
  <!--  -->
  <div :style="{ backgroundColor: bgColor, color: textColor }">
    <div
      v-if="itemsArray.length > 0"
      class="w-full container-nop px-2 flex flex-col md:flex-row items-center"
    >
      <!--  -->
      <div
        v-for="(item, index) in itemsArray"
        :key="index"
        class="w-full p-2 group relative overflow-hidden rounded-xl"
      >
        <!--  -->
        <div
          class="block py-3 px-4 bg-white/10 backdrop-blur-lg rounded-lg transition-all duration-300"
        >
          <!--  -->
          <div class="flex items-center flex-col text-center gap-4">
            <!--  -->
            <div
              class="relative flex-shrink-0 w-14 h-14 rounded overflow-hidden bg-white/10"
            >
              <!--  -->
              <ImageLoader
                :src="
                  item.image ? item.image.src : $store.defaults.ServiceImage
                "
                :alt="item.title"
                img-class="w-full h-full object-contain"
              />
              <!--  -->
            </div>
            <!--  -->

            <!--  -->
            <div class="flex flex-col gap-1">
              <!--  -->
              <span class="font-[600] text-sm uppercase">
                {{ item.title }}
              </span>
              <!--  -->

              <!--  -->
              <span class="text-sm font-extralight">{{
                item.description
              }}</span>
              <!--  -->
            </div>
            <!--  -->
          </div>
          <!--  -->
        </div>
        <!--  -->
      </div>
      <!--  -->
      <!--  -->
    </div>
  </div>
  <!--  -->
</template>
