<script setup>
import { Form } from "vee-validate";
import * as Yup from "yup";

function handleResetPassword(value) {
  console.log(value);
  alert("Password Changed SuccessFully");
}
const schema = Yup.object().shape({
  password: Yup.string().min(6).required("This Field is Required"),
  confirm_password: Yup.string()
    .required("This Field is Required")
    .oneOf([Yup.ref("password")], "Passwords do not match"),
});
</script>
<template>
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
          <h2 class="font-bold text-[#121317] text-2xl mb-1">Reset Password</h2>
          <p class="text-base text-[#404555]">
            Enter new password you haven’t used before
          </p>
        </div>
        <PasswordField
          name="password"
          label="Password"
          placeholder="New Password"
          default-message="Password must be at least 8 characters"
        />
        <PasswordField
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
</template>
