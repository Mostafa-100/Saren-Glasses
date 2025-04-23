<script setup lang="ts">
const { $settings } = useNuxtApp();

const footer = $settings.sections.footer;
const year = new Date().getFullYear();

const copyright = $settings.sections.copyright;
const description = copyright.text;

const bgColor = copyright.colors.background_color;
const textColor = copyright.colors.text_color;

// Add currencies & languages

const $store = useMainStore();

const header = $settings.sections.header;

const activeId = ref(null);
const storeLanguages = ref($settings.store_languages);
const storeCurrencies = ref($settings.store_currencies);

const dropdownMenus = computed(() => {
  return [
    {
      _id: "lang",
      text: $store.language.code,
      active: header.language.active,
      childrens: storeLanguages.value.map((l: any) => ({
        _id: l.code,
        text: l.name,
        url: `?lang=${l.code}`,
      })),
    },
    {
      _id: "currency",
      text: $store.currency.code,
      active: header.currency.active,
      childrens: storeCurrencies.value.map((c: any) => ({
        _id: c.code,
        text: c.name,
        url: `?cur=${c.code}`,
      })),
    },
  ].filter((item) => item.active);
});
</script>

<template>
  <!--  -->
  <div class="flex flex-col text-center md:text-start">
    <!--  -->
    <div
      class="border-t"
      :style="{ backgroundColor: bgColor, color: textColor }"
    >
      <!--  -->
      <div v-if="description.length > 0" class="container py-2">
        <div class="flex justify-between items-center">
          <!--  -->
          <div
            class="text-sm font-light"
            v-html="description.replace(/__YEAR__/gi, year)"
          ></div>
          <!--  -->

          <div
            v-if="header.language.active || header.currency.active"
            class="flex gap-2 items-center"
          >
            <!--  -->
            <div
              v-for="(item, i) in dropdownMenus"
              :key="i"
              class="flex flex-col relative px-2"
            >
              <!--  -->
              <div
                class="flex items-center justify-between cursor-pointer"
                @click="activeId = activeId != item._id ? item._id : null"
              >
                <!--  -->
                <span class="font-light text-sm">{{ item.text }}</span>
                <!--  -->

                <!--  -->
                <Icon
                  v-if="item.childrens.length > 0"
                  name="solar:alt-arrow-down-linear"
                  class="text-sm translate"
                  :class="[
                    activeId == item._id
                      ? 'rotate-180 transition-all duration-300'
                      : '',
                  ]"
                />
                <!--  -->
              </div>
              <!--  -->

              <!--  -->
              <transition name="slide-down">
                <!--  -->
                <div v-if="item._id == activeId">
                  <!--  -->
                  <div v-for="(item2, index) in item.childrens" :key="index">
                    <!--  -->
                    <div
                      class="flex items-center cursor-pointer absolute top-full min-w-[200px] px-2 z-[999] left-0"
                    >
                      <!--  -->
                      <a class="font-light text-sm" :href="item2.url">
                        {{ item2.text }}
                      </a>
                      <!--  -->
                    </div>
                    <!--  -->

                    <!--  -->
                    <div v-for="(child, ii) in item2.childrens" :key="ii">
                      <!--  -->
                      <div
                        class="flex items-center justify-between cursor-pointer"
                      >
                        <!--  -->
                        <a class="font-light text-sm" :href="child.url">
                          {{ child.text }}
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
              </transition>
              <!--  -->
            </div>
            <!--  -->
          </div>
          <FooterPayments :methods="footer.methods" />
        </div>
      </div>
      <!--  -->
    </div>

    <!--  -->
    <div class="flex items-center justify-center py-2 bg-gray-800">
      <!--  -->
      <small class="text-base font-normal text-white">
        Made by
        <!--  -->
        <a
          class="font-bold text-blue-600"
          href="//www.storeino.com/"
          target="_blank"
          >Storeino.com</a
        >
        <!--  -->
      </small>
      <!--  -->
    </div>
    <!--  -->
  </div>
  <!--  -->
</template>
