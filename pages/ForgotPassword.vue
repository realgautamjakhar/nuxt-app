<script setup>
import { Form } from "vee-validate";
import * as Yup from "yup";

const schema = Yup.object().shape({
  email: Yup.string()
    .required("This field is required")
    .email("Please enter a valid email address"),
});
</script>

<template>
  <Modal v-if="modalToggle" />
  <!-- Left hand side informatin -->
  <section
    class="h-screen grid p-5 lg:grid lg:grid-cols-[minmax(auto,526px)_auto] lg:p-0"
  >
    <section class="bg-[#404555] hidden grid-rows-[auto_300px] w-auto lg:grid">
      <CompanyInfo />
    </section>
    <!-- Forget Password Screen  -->
    <section class="flex flex-col items-center justify-around mb-3 mt-5">
      <section
        class="lg:w-[384px] lg:max-h-[294] flex flex-col justify-around h-full w-full lg:h-auto"
      >
        <Form
          class="flex flex-col justify-between h-full w-auto"
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
            <InputCustomField
              name="email"
              type="text"
              label="email"
              placeholder="Registered Email"
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
                class="w-full font-semibold text-lg py-[11px] border-2 border-[#A9871E] bg-[#FEFCF4] rounded"
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
      const { email } = value;
      let response = await fetch(
        "https://project-api-01.herokuapp.com/user/findMailId",
        {
          method: "post",
          body: JSON.stringify({
            email: email.toLowerCase(),
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
