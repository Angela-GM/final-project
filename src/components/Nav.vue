<template>
  <nav>
    <img
      id="logo"
      src="https://img.icons8.com/clouds/300/null/todo-list.png"
      alt="logo imagen"
    />
    <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link" />
    <!-- <router-link to="/"> Home </router-link> -->

    <ul>
      <!-- <li>
        <router-link to="/">Task Manager</router-link>
      </li> -->

      <li>
        <router-link to="/account">Your Account</router-link>
      </li>
    </ul>

    <div>
      <ul class="log-out">
        <li>
          <!-- <img
            class="profile-img"
            id="img-nav"
            :src="
              avatar_url
                ? avatar_url
                : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
            "
            alt="Profile picture"
          /> -->
        </li>
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
import { ref, onMounted } from "vue";

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Task Time";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;
// declarar variable de la UserStore
const userStore = useUserStore();

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;
const avatar_url = ref(null);

// Ejecutar la funcion getProfile al cargar la página
onMounted(() => {
  getProfile();
});

// Función para traer los datos del perfil desde la store
async function getProfile() {
  await userStore.fetchUser();
  avatar_url.value = userStore.profile.avatar_url;
}

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

<style></style>
