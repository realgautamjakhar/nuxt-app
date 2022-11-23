<script setup>
import { Form } from "vee-validate";
import * as Yup from "yup";

const schema = Yup.object().shape({
  password: Yup.string().min(6).required("This Field is Required"),
  confirm_password: Yup.string()
    .required("This Field is Required")
    .oneOf([Yup.ref("password")], "Passwords do not match"),
});
</script>

<template>
  <Notify
    v-if="showNotification"
    message="Your password is reset successfully"
  />
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-5 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <CompanyInfo />
    </section>
    <!-- Reset Password Screen  -->
    <section class="flex flex-col items-center justify-around mb-3 mt-5">
      <section
        class="lg:w-[384px] lg:max-h-[294] flex flex-col justify-around h-full lg:h-auto"
      >
        <Form
          class="flex flex-col justify-between h-full my-8 w-auto"
          @submit="handleResetPassword"
          :validation-schema="schema"
        >
          <div class="flex flex-col">
            <img
              src="../assets/logos/mobilelogo.svg"
              class="w-[150px] h-[42px] lg:hidden mb-10 self-center"
            />
            <div class="mb-6">
              <h2 class="font-bold text-[#121317] text-2xl mb-1">
                Reset Password
              </h2>
              <p class="text-base text-[#404555]">
                Enter new password you haven’t used before
              </p>
            </div>
            <PasswordCustomField
              name="password"
              label="Password"
              placeholder="New Password"
              default-message="Password must be at least 8 characters"
            />
            <PasswordCustomField
              name="confirm_password"
              label="Password"
              placeholder="Confirm New Password"
              default-message="Both passwords must match!"
              class="mt-4"
            />
          </div>
          <div>
            <button
              type="submit"
              class="submit-btn w-full bg-[#F1C12B] rounded font-semibold text-lg py-[11px] mt-6"
            >
              Reset Password
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
      email: "",
      showNotification: false,
    };
  },
  created() {
    this.email = this.$route.query.email;
    //Send ID or email to this as a link params and populate here
  },
  methods: {
    handleResetPassword(value) {
      const userEmail = this.email;
      const { password } = value;
      this.showNotification = true;
    },
  },
};
</script>
