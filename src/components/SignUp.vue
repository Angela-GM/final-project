<template>
  <div class="container">
    <div class="header">
      <div class="header-description">
        <!-- Logo -->
        <figure>
          <!-- imagen del logo -->
          <img
            src="https://img.icons8.com/clouds/300/null/todo-list.png"
            alt="icon to do list"
          />
        </figure>
        <h3 class="header-title">Register to TaskTime</h3>
        <p class="header-subtitle">Here starts your day!</p>
      </div>
    </div>

    <form @submit.prevent="signUp" class="form-sign-in">
      <div class="form">
        <div class="form-input">
          <label class="input-field-label">E-mail</label>
          <input
            type="email"
            class="input-field"
            placeholder="example@gmail.com"
            id="email"
            v-model="email"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label">Password</label>
          <div class="form-container-input-icon">
            <input
            :type="showPassword ? 'text' : 'password'"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
          <!-- Boton/imagen mostrar/ocultar contraseña -->
          <img
            @click="showPassword = !showPassword"
            class="icon-img img-eye"
            :src="showPassword ? eyeImageShow : eyeImageNoShow"
            alt="icon eye"
          />

          </div>
          
        </div>
        <div class="form-input">
          <label class="input-field-label">Confirm password</label>
          <div class="form-container-input-icon">
            <input
            :type="showPassword ? 'text' : 'password'"
            class="input-field"
            placeholder="**********"
            id="confirmPassword"
            v-model="confirmPassword"
            required
          />
          <!-- Boton/imagen mostrar/ocultar contraseña -->
          <img
            @click="showPassword = !showPassword"
            class="icon-img img-eye"
            :src="showPassword ? eyeImageShow : eyeImageNoShow"
            alt="icon eye"
          />
          </div>
        </div>
        <button class="button" type="submit">Sign Up</button>
        <p>
          Have an account?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>
      </div>
    </form>

    <div v-show="errorMsg">{{ errorMsg }}</div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/login";
const buttonText = "Sign In";

// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");

// variables para mostrar contraseña
const showPassword = ref(false);
const eyeImageNoShow = ref(
  "https://img.icons8.com/material-outlined/24/null/visible--v1.png"
);
const eyeImageShow = ref(
  "https://img.icons8.com/material-outlined/24/null/hide.png"
);
// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signUp(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/auth/login" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "error";
};
</script>

<style></style>
