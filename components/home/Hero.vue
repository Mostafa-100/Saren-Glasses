<script setup>
import { useNuxtApp } from "#app";
import { ref } from "vue";

const { $settings } = useNuxtApp();
const hero = $settings?.sections?.hero;

const image = ref(hero?.image ?? null);

const description = ref(hero?.description ?? "");
const header = ref(hero?.header ?? "");

const buyNowButtonText = ref(hero?.buy_now_button_text ?? "");
const buyNowButtonUrl = ref(hero?.buy_now_button_url ?? "");

const learnMoreButtonText = ref(hero?.learn_more_button_text ?? "");
const learnMoreButtonUrl = ref(hero?.learn_more_button_url ?? "");
</script>

<template>
  <div class="relative h-lvh">
    <img
      v-if="image"
      :src="image.src"
      class="absolute top-0 left-0 w-full h-full object-cover"
    />
    <div
      class="absolute w-full bottom-10 flex justify-between flex-col lg:flex-row px-7"
    >
      <GlobalMainText
        :content="description"
        otherClass="w-auto lg:w-[380px] order-2 lg:order-1"
      />
      <GlobalSectionHeader
        :content="header"
        otherClass="w-auto lg:w-[270px] order-1 lg:order-2s"
      />
      <GlobalSectionLink :label="buyNowButtonText" :link="buyNowButtonUrl" />
      <GlobalSectionLink
        :label="learnMoreButtonText"
        :link="learnMoreButtonUrl"
      />
    </div>
  </div>
</template>
