<script setup>
import { toRef } from "vue";
import { useField } from "vee-validate";

const props = defineProps({
  type: {
    type: String,
    default: "text",
  },
  value: {
    type: String,
    default: "",
  },
  name: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  placeholder: {
    type: String,
    default: "",
  },
  defaultMessage: {
    type: String,
    default: "",
  },
});
const name = toRef(props, "name");
//Usefield property that handle changes and error if any error in field than has error class style will get executed.. meta (flag for field status)
const {
  value: inputValue,
  errorMessage,
  handleChange,
  meta,
} = useField(name, undefined, {
  initialValue: props.value,
});
</script>

<template>
  <div class="relative">
    <input
      :name="name"
      :id="name"
      :type="type"
      :value="inputValue"
      :placeholder="placeholder"
      @input="handleChange"
      class="border-[#DCDEE5] border-2 outline-none py-3 px-4 text-[16px] w-full rounded"
      :class="{ 'has-error': !!errorMessage }"
    />
    <p
      v-show="!errorMessage && !meta.valid"
      class="text-[12px] text-[#606880] mt-1"
    >
      {{ defaultMessage }}
    </p>
    <p class="text-[14px] text-red-400" v-show="errorMessage || meta.valid">
      {{ errorMessage }}
    </p>
  </div>
</template>

<style>
.has-error {
  border-color: red;
  color: red;
}
.success {
  border-color: green;
}
</style>
