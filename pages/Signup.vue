<script setup>
import { Form } from "vee-validate";
import * as Yup from "yup";
const schema = Yup.object().shape({
  fname: Yup.string().required("This Field is Required"),
  password: Yup.string()
    .min(8, "Password must be at least 8 characters")
    .required("This Field is Required"),
});
</script>

<template>
  <Notify v-if="showNotification" message="Signed Up Successfully" />
  <!-- Left hand side informatin -->
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-5 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <Info />
    </section>
    <!-- Right hand side Forms  -->
    <section class="flex flex-col items-center justify-around mb-3">
      <section
        class="lg:w-[384px] flex flex-col justify-around h-full lg:h-auto"
      >
        <Form
          class="flex flex-col justify-between h-full w-auto"
          @submit="handleSignup"
          :validation-schema="schema"
        >
          <div class="flex flex-col">
            <img
              src="../assets/logos/mobilelogo.svg"
              class="w-[150px] h-[42px] lg:hidden mb-10 self-center"
            />
            <h2 class="font-bold text-2xl mb-1 text-[#121317]">
              Create an account
            </h2>
            <p class="text-base mb-6 text-[#404555]">
              Already have an account?
              <NuxtLink to="/" class="font-semibold text-[#2558E5]"
                >Sign In</NuxtLink
              >
            </p>
            <InputCustom
              name="fname"
              type="text"
              label="fname"
              placeholder="First Name"
            />
            <InputCustom
              name="lname"
              type="text"
              label="lname"
              placeholder="Last Name"
              class="mt-4"
            />
            <InputCustom
              name="email"
              type="text"
              label="email"
              placeholder="Email Address"
              class="mt-4"
            />
            <div class="grid grid-cols-[88px_auto] gap-2 items-center mt-4">
              <div
                class="border-[#DCDEE5] border-2 flex items-center justify-between h-full py-3 px-4 text-[16px] w-full rounded text-center"
              >
                <img
                  src="../assets/logos/indiaflag.svg"
                  class="w-[24px] h-[16px]"
                  alt="indianFlag"
                />
                <img
                  src="../assets/logos/downarrow.svg"
                  class="w-[11.36px] h-[6.47px]"
                  alt="indianFlag"
                />
              </div>
              <InputCustom
                name="mobile"
                type="number"
                label="mobile"
                placeholder="Mobile Number"
              />
            </div>
            <PasswordField
              name="password"
              label="Password"
              placeholder="Password"
              default-message="Password must be at least 8 characters"
              class="mt-4"
            />
            <p class="text-xs mb-6 mt-4 text-[#404555]">
              By clicking Sign Up you are indicating that you have read and
              acknowledged the
              <NuxtLink to="/" class="text-[#2558E5]"
                >Terms of Service</NuxtLink
              >
              and
              <NuxtLink to="/" class="text-[#2558E5]">Privacy Notice</NuxtLink>
            </p>
          </div>
          <div>
            <button
              type="submit"
              class="w-full bg-[#F1C12B] font-semibold text-[#121317] text-lg py-[11px] mb-4 rounded"
            >
              Sign Up
            </button>
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
      showNotification: false,
    };
  },
  methods: {
    async handleSignup(value) {
      const { fname, lname, mobile, email, password } = value;
      let result = await fetch(
        "https://project-api-01.herokuapp.com/user/register",
        {
          method: "post",
          body: JSON.stringify({
            lname,
            mobile,
            email,
            password,
            fname,
          }),
          headers: {
            "Content-Type": "application/json",
          },
        }
      );
      result = await result.json();
      if (result.auth) {
        localStorage.setItem("token", JSON.stringify(result.auth));
        this.showNotification = true;
      }
    },
  },
};
</script>
