<template>
  <section
    v-if="toggle"
    class="z-10 fixed w-full h-full flex justify-center items-center bg-[rgba(0,0,0,0.3)]"
  >
    <div class="bg-[white] max-w-[486px] h-[170px] m-2 p-5 border-2 rounded-lg">
      <div class="flex justify-between mb-4">
        <h2 class="font-bold text-2xl mb-1 text-[#121317]">Reset Password</h2>
        <img
          src="../assets/logos/cross.svg"
          alt=""
          class="max-w-[32px] max-h-[32px] cursor-pointer"
          @click="closeModal"
        />
      </div>
      <hr />
      <p class="mt-4 text-[#404555]">
        Thank you! We have send you a link to reset your account password.Please
        check your email
      </p>
    </div>
  </section>
</template>
<script>
export default {
  data() {
    return {
      toggle: true,
    };
  },
  methods: {
    closeModal() {
      this.toggle = false;
    },
  },
};
</script>
