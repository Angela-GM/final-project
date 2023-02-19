<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="container">
    <!-- Logo de la web -->
    <div class="logo">
      <figure>
        <!-- imagen del logo -->
        <img
          src="https://img.icons8.com/clouds/300/null/todo-list.png"
          alt="icon to do list"
        />
      </figure>
      <!-- Titulo -->
      <h1>Welcome to TaskTime</h1>
      <h3>Here starts your day!</h3>
      <h4>Sign In</h4>
    </div>

    <!-- Formulario SignIn -->
    <form @submit.prevent="signIn" class="form-sign-in">
      <div class="form">
        <div class="form-input">
          <label class="input-field-label" v-show="none">E-mail</label>
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
          <label class="input-field-label" v-show="none">Password</label>
          <!-- Input password -->
          <div class="form-container-input-icon">
            <input
              class="input-field"
              placeholder="**********"
              id="password"
              required
              :type="showPassword ? 'text' : 'password'"
              v-model="password"
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

        <button class="button" type="submit">Sign In</button>
      </div>
    </form>

    <span>
      Dont have an account?

      <PersonalRouter
        :route="route"
        :buttonText="buttonText"
        class="sign-up-link"
    /></span>
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
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");
const redirect = useRouter();

// Variables para mostrar contrasña
const showPassword = ref(false);
const eyeImageNoShow = ref(
  "https://img.icons8.com/material-outlined/24/null/visible--v1.png"
);
const eyeImageShow = ref(
  "https://img.icons8.com/material-outlined/24/null/hide.png"
);

// Arrow function to Signin user to supaBase
const signIn = async () => {
  if (email.value) {
    try {
      await useUserStore().signIn(email.value, password.value);
      redirect.push({ path: "/" });
    } catch (error) {
      console.error(error);
      throw error;
    }
  }
};
</script>

<style></style>
