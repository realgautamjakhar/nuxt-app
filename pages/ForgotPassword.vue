<script setup>
import { Form } from "vee-validate";
import * as Yup from "yup";

const schema = Yup.object().shape({
  email: Yup.string().required().email("Please Enter a valid Email"),
});
</script>

<template>
  <!-- Modal  -->
  <section
    v-if="modalToggle"
    class="z-10 fixed w-full h-full flex justify-center items-center bg-[rgba(0,0,0,0.3)]"
  >
    <div class="bg-[white] max-w-[486px] h-[170px] m-2 p-5 border-2 rounded-lg">
      <div class="flex justify-between mb-4">
        <h2 class="font-bold text-2xl mb-1 text-[#121317]">Reset Password</h2>
        <img
          src="../assets/logos/cross.svg"
          alt=""
          class="max-w-[32px] max-h-[32px] cursor-pointer"
          @click="closeToggleModal"
        />
      </div>
      <hr />
      <p class="mt-4 text-[#404555]">
        Thank you! We have send you a link to reset your account password.Please
        check your email
      </p>
    </div>
  </section>

  <!-- Left hand side informatin -->
  <section
    class="h-screen grid p-4 lg:grid lg:grid-cols-[minmax(auto,526px)_auto] lg:p-0"
  >
    <section class="bg-[#404555] hidden grid-rows-[auto_300px] w-auto lg:grid">
      <Info />
    </section>
    <!-- Right hand side Forms  -->
    <section class="flex flex-col items-center justify-around">
      <section
        class="lg:w-[384px] lg:max-h-[294] flex flex-col justify-around h-full w-full lg:h-auto"
      >
        <Form
          class="flex flex-col justify-between h-full my-8 w-auto"
          @submit="findMailId"
          :validation-schema="schema"
        >
          <div class="flex flex-col">
            <img
              src="../assets/logos/mobilelogo.svg"
              class="w-[150px] h-[42px] lg:hidden mb-10 self-center"
            />
            <h2 class="font-bold text-2xl mb-1 text-[#121317]">
              Forgot Password
            </h2>
            <p class="text-base mb-6 text-[#404555]">
              We’ll send you a reset password link to your registered email
              address
            </p>
            <InputCustom
              name="email"
              type="text"
              label="email"
              placeholder="Email Address"
            />
            <p
              v-if="invalidMail"
              class="text-[14px] text-[#D92D20] font-normal"
            >
              Sorry! This email is not registered.
            </p>
          </div>
          <div>
            <button
              type="submit"
              class="w-full bg-[#F1C12B] font-semibold text-lg py-[11px] mt-6 mb-4 rounded"
            >
              Email me a recovery link
            </button>
            <NuxtLink to="/">
              <button
                type="submit"
                class="w-full font-semibold text-lg py-[11px] border-2 border-[#A9871E] bg[#FEFCF4] rounded"
              >
                Return to sign In
              </button>
            </NuxtLink>
          </div>
        </Form>
      </section>
    </section>
  </section>
</template>

<script>
export default {
  data() {
    return {
      modalToggle: false,
      invalidMail: false,
      error: "",
    };
  },
  methods: {
    closeToggleModal() {
      this.modalToggle = false;
    },
    async findMailId(value) {
      console.log(value);
      const { email } = value;
      console.log(email);
      let response = await fetch(
        "https://nuxt-project-api.herokuapp.com/findMailId",
        {
          method: "post",
          body: JSON.stringify({
            email,
          }),
          headers: {
            "Content-Type": "application/json",
          },
        }
      );
      response = await response.json();
      if (response.validMail === true) {
        this.invalidMail = false;
        this.modalToggle = true;
      } else if (response.validMail === false) {
        this.invalidMail = true;
      }
    },
  },
};
</script>
