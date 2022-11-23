<script setup>
const props = defineProps({
  message: {
    type: String,
    default: "",
  },
});
</script>

<template>
  <div
    v-if="toggle"
    class="bg-white absolute max-h-[64px] top-0 right-0 p-4 m-4 flex gap-3 items-center border-l-4 border-[#039855] rounded-md shadow-md z-10"
  >
    <img
      src="../assets/logos/circlecheck.svg"
      alt="SuccessFully login"
      class="max-w-[20px] max-h-[20px] z-0"
    />
    <h2 class="text-sm">{{ props.message }}</h2>
    <img
      src="../assets/logos/v1cross.svg"
      alt="close logo"
      class="cursor-pointer"
      @click="closeNotification"
    />
  </div>
</template>
<script>
export default {
  data() {
    return {
      toggle: true,
    };
  },
  methods: {
    closeNotification() {
      this.toggle = false;
    },
  },
};
</script>
