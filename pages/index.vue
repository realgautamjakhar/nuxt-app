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
      return phone && phone.toString().length == 10 ? true : false;
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
    .min(8, "Password Must be 8 Character")
    .required("Password is Required"),
});
</script>

<template>
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-5 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <CompanyInfo />
    </section>
    <!-- Login Form Screen -->
    <section class="flex flex-col items-center justify-around mb-3 mt-5">
      <Notify v-if="showNotification" message="Signed In Successfully" />
      <section
        class="flex flex-col justify-around h-full w-full lg:w-[384px] lg:max-h-[294px] lg:h-auto"
      >
        <Form
          class="flex flex-col justify-between h-full w-auto"
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
            <InputCustomField
              name="email_or_phone"
              type="text"
              label="email_or_phone"
              placeholder="Email or Mobile Number"
            />
            <p
              v-if="invalidInput"
              class="text-[14px] text-[#D92D20] font-normal"
            >
              {{ error }}
            </p>

            <!-- Password Field  -->
            <PasswordCustomField
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
            Sign in
          </button>
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
    async handleSignIn(value) {
      const { email_or_phone, password } = value;
      try {
        let result = await fetch(
          "https://project-api-01.herokuapp.com/user/login",
          {
            method: "post",
            body: JSON.stringify({
              loginInput: email_or_phone.toLowerCase(),
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
          this.error = result.mobileNotRegistered;
        } else if (result.invalidPassword) {
          this.invalidPassword = true;
          this.error = result.invalidPassword;
        } else {
          this.invalidInput = false;
          this.invalidPassword = false;
          this.showNotification = true;
        }
        return result;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
