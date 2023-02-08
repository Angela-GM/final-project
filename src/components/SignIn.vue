<!-- COMPONENTE BOILERPLATE -->

<template>
  <div class="container">
    <!-- <h3 class="header-title">Log In to ToDo App</h3>
    <p class="header-subtitle">Estamos en la ruta de login. Aquí deberíais crear un form con la lógica correspondiente para que este permita al usuario loguearse con su email y su contraseña. Miraros la lógica de SignUp si necesitáis inspiración :)</p>
    <p>Dont have an account? <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link"/></p> -->

<!-- Logo de la web -->
    <figure>Logo</figure>
    <h1>Welcome to TaskTime</h1>
    <h3>Here starts your day.</h3>
    <h3>Sign In</h3>

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
          <input
            type="password"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
        </div>
        
        <button class="button" type="submit">Sign In</button>
        
      </div>
    </form>

    <p>Dont have an account? <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link"/></p>

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


// Arrow function to Signin user to supaBase
const signIn = async () => {
  if (email.value){
    try {
      await useUserStore().signIn(email.value, password.value);
      redirect.push({ path: "/" });
      
  } catch (error) {
    console.error(error);
    throw error;
  }
  };

};


</script>

<style></style>
