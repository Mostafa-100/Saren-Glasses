<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useMainStore } from "@/stores/main";
import { useNuxtApp } from "#app";

const { $settings }: any = useNuxtApp();
const $store = useMainStore();

const header = $settings.sections.header;

const wishlist = ref(header.wishlist);
const cart = ref(header.cart);
const account = ref(header.account);

const desktopLogo = ref(header.logo.desktop);
const mobileLogo = ref(header.logo.mobile);

const bgColor = ref(header.colors.background_color);

const currentBgColor = "transparent";

const textColor = ref(header.colors.text_color);

const searchBgColor = ref(header.search.background_color);
const currentSearchBgColor = "transparent";

const searchTextColor = ref(header.search.text_color);

const customer: any | string = ref(null);

const firstLetters = (firstName: string, lastName: string) => {
  return `${firstName[0]}.${lastName[0]}`.toUpperCase();
};

onMounted(() => {
  customer.value = $store.customer?.customer || null;
});

// Add currencies and languages in header
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
  <div class="header absolute top-0 w-full z-10">
    <!--  -->
    <appsLoader placement="BEFORE_HEADER" />
    <!--  -->

    <!--  -->
    <div
      class="z-50"
      :style="{
        backgroundColor: currentBgColor,
        color: textColor,
      }"
    >
      <!-- Header -->
      <div class="header py-4">
        <!--  -->
        <header
          class="container flex flex-wrap lg:flex-nowrap items-center justify-between lg:gap-6 py-2 px-4"
        >
          <!-- Left Content -->
          <div
            class="w-1/2 lg:w-1/4 flex order-1 lg:order-none items-center justify-start gap-4"
          >
            <!-- header Menu -->
            <div
              class="flex items-center justify-center lg:hidden cursor-pointer"
              aria-label="Show header_menu"
              @click="$store.showHeaderMenu = !$store.showHeaderMenu"
            >
              <!--  -->
              <Icon
                name="solar:hamburger-menu-linear"
                class="text-3xl translate"
              />
              <!--  -->
            </div>
            <!-- header Menu -->

            <!-- logo -->
            <div class="logo">
              <HeaderLogo
                :mobile-logo="mobileLogo"
                :desktop-logo="desktopLogo"
                :default-logo="$store.defaults.logo"
              />
            </div>
            <!-- logo -->
          </div>
          <!-- Left Content -->

          <!-- Center Content -->
          <div
            class="w-full lg:w-2/4 flex order-3 lg:order-none items-center justify-center mt-4 lg:mt-0"
          >
            <!--  Search -->
            <HeaderSearch
              :is-active="header.search.active"
              :placeholder="header.search.text"
              :search-bg-color="currentSearchBgColor"
              :search-text-color="searchTextColor"
            />
            <!-- Search -->
          </div>
          <!-- Center Content -->

          <!-- Right Content -->

          <div
            class="flex w-1/2 lg:w-1/4 order-2 lg:order-none items-center justify-end lg:justify-around"
          >
            <div
              v-if="header.language.active || header.currency.active"
              class="hidden lg:flex gap-2 items-center"
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
            <!-- Icons -->
            <div class="flex items-center justify-end gap-4 lg:gap-6">
              <!-- Wishlist -->
              <HeaderIcons
                link="/wishlist"
                title="Wishlist"
                icon-name="solar:heart-linear"
                :is-active="wishlist.active"
                :badge-count="$store.wishlist.length"
                :text="wishlist.text"
                :text-color="textColor"
                :bg-color="bgColor"
              />
              <!-- Wishlist -->

              <!-- Cart -->
              <HeaderIcons
                link="/cart"
                title="Cart"
                icon-name="solar:cart-large-2-linear"
                :is-active="cart.active"
                :badge-count="$store.cart.length"
                :text="cart.text"
                :text-color="textColor"
                :bg-color="bgColor"
              />
              <!-- Cart -->

              <!-- Account -->
              <HeaderIcons
                :link="customer ? '/account/orders' : '/account/login'"
                title="Account"
                icon-name="solar:user-outline"
                :is-active="account.active"
                :text="
                  customer
                    ? firstLetters(customer.firstname, customer.lastname)
                    : account.text
                "
              />
              <!-- Account -->
            </div>
            <!--  -->
          </div>
          <!-- Right Content -->
        </header>
        <!--  -->
      </div>
      <!-- Header -->

      <!-- Header Menu -->
      <div class="header">
        <!--  -->
        <HeaderMenu :bg-color="currentBgColor" :text-color="textColor" />
        <!--  -->
      </div>
      <!-- Header Menu -->
    </div>

    <!--  -->
    <appsLoader placement="AFTER_HEADER" />
    <!--  -->
  </div>
  <!--  -->
</template>
