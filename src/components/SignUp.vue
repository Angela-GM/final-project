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

  <!-- Ventana Modal al eliminar una tarea -->
  <!-- Overlay -->
  <!-- <transition name="fade">
    <div class="modal-overlay" v-if="showModalConfirmation"></div>
  </transition>

  <transition name="fade">
    <div class="modal-container" v-if="showModalConfirmation">
      <h1>Welcome to TaskTime</h1>
      <button @click="!showModalConfirmation">Cerrar</button>
    </div>
  </transition> -->
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

// Modal de confirmación
// const showModalConfirmation = ref(false);

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signUp(email.value, password.value);

      //hay que añadir un modal para informar al usuario que debe confirmar su correo
      //debe cambiar el valor de una variable de false a true
      // showModalConfirmation = true;

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
