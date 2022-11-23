<script setup>
import * as Yup from "yup";
import { Form } from "vee-validate";

const validateEmail = (email) => {
  return Yup.string().email().isValidSync(email);
};

const validatePhone = (phone) => {
  return Yup.number()
    .integer()
    .positive()
    .test((phone) => {
      return phone &&
        phone.toString().length >= 8 &&
        phone.toString().length <= 14
        ? true
        : false;
    })
    .isValidSync(phone);
};

const schema = Yup.object().shape({
  email_or_phone: Yup.string()
    .required("Email / Phone is required")
    .test("email_or_phone", "Email / Phone is invalid", (value) => {
      return validateEmail(value) || validatePhone(parseInt(value ?? "0"));
    }),
  password: Yup.string()
    .min(6, "Password Must be 6 Character")
    .required("Password is Required"),
});
</script>

<template>
  <div
    v-if="loginSuccess"
    class="bg-white absolute max-h-[64px] top-0 right-0 p-4 m-4 flex gap-3 items-center border-l-4 border-[#039855] rounded-md shadow-md z-20"
  >
    <img
      src="../assets/logos/circlecheck.svg"
      alt="SuccessFully login"
      class="max-w-[20px] max-h-[20px] z-0"
    />
    <h2 class="text-sm">You Signed In successfully</h2>
    <img
      src="../assets/logos/v1cross.svg"
      alt="close logo"
      class="cursor-pointer"
      @click="loginNotification"
    />
  </div>
  <section
    class="lg:w-[384px] lg:max-h-[294px] flex flex-col justify-around h-full w-full lg:h-auto"
  >
    <Form
      class="flex flex-col justify-between h-full my-8 w-auto"
      @submit="handleSignIn"
      :validation-schema="schema"
    >
      <div class="flex flex-col">
        <img
          src="../assets/logos/mobilelogo.svg"
          class="w-[150px] h-[42px] lg:hidden mb-10 self-center"
        />
        <h2 class="font-bold text-2xl mb-1 text-[#121317]">
          Sign In to WisdomCircle
        </h2>
        <p class="text-base mb-6 text-[#404555]">
          Don’t have an account?
          <NuxtLink to="/signup" class="font-semibold text-[#2558E5]"
            >Sign Up</NuxtLink
          >
        </p>

        <!-- Email/mobile InputField  -->
        <InputCustom
          name="email_or_phone"
          type="text"
          label="email_or_phone"
          placeholder="Email or Mobile Number"
        />
        <p v-if="invalidInput" class="text-[14px] text-[#D92D20] font-normal">
          {{ error }}
        </p>

        <!-- Password Field  -->
        <PasswordField
          name="password"
          label="Password"
          placeholder="Password"
          class="mt-4"
        />
        <p
          v-if="invalidPassword"
          class="text-[14px] text-[#D92D20] font-normal"
        >
          {{ error }}
        </p>
        <p
          class="font-semibold mb-6 text-[#2558E5] self-end hover:cursor-pointer leading-6 mt-1"
        >
          <NuxtLink to="/forgotpassword">Forgot password</NuxtLink>
        </p>
      </div>

      <button
        type="submit"
        class="w-full font-semibold text-lg py-[11px] rounded bg-[#F1C12B]"
      >
        <h1 v-if="!loginSuccess">Sign in</h1>
        <h2 v-else class="flex items-center justify-center gap-4">
          Signed In
          <img
            src="../assets/logos/circlecheck.svg"
            alt="SuccessFully login"
            class="max-w-[20px] max-h-[20px]"
          />
        </h2>
      </button>
    </Form>
  </section>
</template>

<script>
export default {
  data() {
    return {
      loginSuccess: false,
      invalidInput: false,
      invalidPassword: false,
      error: "",
    };
  },
  computed: {
    buttonLabel() {
      return this.showPassword ? "Hide" : "Show";
    },
  },
  methods: {
    switchVisibility() {
      this.passwordFieldType =
        this.passwordFieldType === "password" ? "text" : "password";
      this.toggle = this.toggle === false ? true : false;
    },
    loginNotification() {
      this.loginSuccess = !this.loginSuccess;
    },
    async handleSignIn(value) {
      const { email_or_phone, password } = value;
      console.log(value);
      console.log(email_or_phone, password);
      try {
        let result = await fetch(
          "https://project-api-01.herokuapp.com/user/login",
          {
            method: "post",
            body: JSON.stringify({
              loginInput: email_or_phone,
              password,
            }),
            headers: {
              "Content-Type": "application/json",
            },
          }
        );
        result = await result.json();
        if (result.emailNotRegistered) {
          this.invalidInput = true;
          this.error = result.emailNotRegistered;
        } else if (result.mobileNotRegistered) {
          this.invalidInput = true;
          console.log(result.mobileNotRegistered);
          this.error = result.mobileNotRegistered;
        } else if (result.invalidPassword) {
          this.invalidPassword = true;
          this.error = result.invalidPassword;
        } else {
          this.invalidInput = false;
          this.invalidPassword = false;
          this.loginNotification();
        }
        return result;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
