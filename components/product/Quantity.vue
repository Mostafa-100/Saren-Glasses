<script setup lang="ts">
const props: any = defineProps({
  quantity: Object,
});

const emit = defineEmits(["selected"]);

const value = ref(props.quantity.value || props.quantity.default);

watch(value, (val) => {
  if (val > props.quantity.instock) value.value = props.quantity.instock;
  if (val < props.quantity.min) value.value = props.quantity.default;
  if (isNaN(val)) value.value = props.quantity.default;
  emit("selected", value.value);
});

const inc = (inc: any) => {
  value.value =
    Number(value.value) + Number(props.quantity.increment) * Number(inc);
};
</script>

<template>
  <!--  -->
  <div class="flex items-center bg-white overflow-hidden">
    <!-- -->
    <button
      class="flex items-center justify-center border rounded-sm p-2"
      @click="inc(-1)"
    >
      <!--  -->
      <Icon name="iconoir:minus" class="text-sm translate" />
      <!--  -->
    </button>

    <!--  -->
    <div class="w-fit flex items-center justify-center">
      <!--  -->
      <span class="text-sm text-center text-[#8E9194] px-5">{{ value }}</span>

      <!--  -->
      <input
        v-model="value"
        :min="props.quantity.min"
        :max="props.quantity.max"
        class="absolute inset-0 w-full h-full text-sm text-center bg-opacity-0 outline-none"
        type="hidden"
      />
      <!--  -->
      <!--  -->

      <!--  -->
      <div v-if="props.quantity.unit" class="flex items-center justify-center">
        <!--  -->
        <span class="text-sm font-medium"> {{ props.quantity.unit }} </span>
        <!--  -->
      </div>
      <!--  -->
    </div>
    <!--  -->

    <!--  -->
    <button
      class="flex items-center justify-center border rounded-sm p-2"
      @click="inc(1)"
    >
      <!--  -->
      <Icon name="iconoir:plus" class="text-sm translate" />
      <!--  -->
    </button>
    <!--  -->
  </div>
  <!--  -->
</template>
