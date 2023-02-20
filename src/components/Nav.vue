<template>
  <nav>
    <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link" />
    <router-link to="/"> Home </router-link>

    <ul>
      <li>
        <router-link to="/">Task Manager</router-link>
      </li>

      <li>
        <router-link to="/account">Your Account</router-link>
      </li>
    </ul>

    <div>
      <ul>
        <li class="log-out-welcome">
          <p>Welcome, {{ userEmail }}</p>
        </li>
        <li>
          <button @click.prevent="signOut" class="button">Log out</button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from "vue";

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  console.log("ha entrado en la funcion signOut");
  try {
    // call the user store and send the users info to backend to signOut
    await useUserStore().signOut();
    // then redirect user to the homeView
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    console.error(error);
    throw error;
  }
};
</script>

<style>
.navbar-img {
  width: 90px;
}

nav {
  font-size: 0.8rem;
  background-color: #c8ede6;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
  color: white;
}

nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
