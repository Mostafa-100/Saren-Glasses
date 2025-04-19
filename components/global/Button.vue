<script setup lang="ts">
const props: any = defineProps({
  variant: {
    type: String,
    default: "primary",
    validator: (value: any) => ["primary", "secondary"].includes(value),
  },
  icon: {
    type: String,
    default: "",
  },
  text: {
    type: String,
    required: true,
  },
  isVisible: {
    type: Boolean,
    default: true,
  },
});

const emit = defineEmits(["click"]);

const handleClick = () => {
  emit("click");
};

const buttonClasses = computed(() => {
  const baseClasses =
    "w-full h-10 flex items-center justify-between px-12 relative click-effect rounded-md transition-colors duration-100";
  const variantClasses =
    props.variant === "primary"
      ? "bg-[#0F0F0F] uppercase hover:opacity-75 text-[#F1F1F1] font-light"
      : "bg-secondary hover:bg-transparent hover:text-secondary";

  return `${baseClasses} ${variantClasses}`;
});
</script>

<template>
  <!--  -->
  <button v-if="isVisible" :class="buttonClasses" @click="handleClick">
    <!--  -->
    <div class="flex justify-center items-center grow">
      <span class="text-sm">{{ text }}</span>
    </div>
    <Icon
      v-if="icon"
      :name="icon"
      class="text-sm text-white right-[8px] top-[13px]"
    />
    <!--  -->
  </button>
  <!--  -->
</template>
