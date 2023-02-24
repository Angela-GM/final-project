<template>
  <!-- Menu Hamburguesa -->
  <span class="nav-bar" @click="toggleMenu">
    <i class="fa-solid fa-bars" id="cruz" :class="{ 'fa-times': visible }"></i>
  </span>

  <nav class="main-nav">
    <ul class="menu" :class="{ mostrar: visible }">
      <li>
        <!-- logo -->
        <div class="logo">
          <router-link to="/"
            ><img
              class="logo"
              id="logo"
              src="https://img.icons8.com/clouds/300/null/todo-list.png"
              alt="logo imagen"
            />
          </router-link>
        </div>
      </li>
      <li class="menu-item">
        <PersonalRouter
          :route="route"
          :buttonText="buttonText"
          class="logo-link"
        />
      </li>
      <li class="menu-item">
        <p v-if="!username">Welcome, {{ userEmail }}</p>
        <p v-else>Welcome, {{ username }}</p>
      </li>
      <li class="menu-item">
        <a href="#" class="menu-link">
          <router-link to="/account">
            <img
              id="img-nav"
              :src="
                avatar_url
                  ? avatar_url
                  : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
              "
              alt="Profile picture"
            />
          </router-link>
        </a>
      </li>
      <li class="menu-item">
        <a href="#" class="menu-link">
          <button @click.prevent="signOut" class="button">Log out</button>
        </a>
      </li>
    </ul>
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
const username = ref(null);

// Ejecutar la funcion getProfile al cargar la página
onMounted(() => {
  getProfile();
});

// Función para traer los datos del perfil desde la store
async function getProfile() {
  await userStore.fetchUser();
  avatar_url.value = userStore.profile.avatar_url;
  username.value = userStore.profile.username;
}

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  // console.log("ha entrado en la funcion signOut");
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

// menu hamburguesa
let visible = ref(false);
const toggleMenu = () => {
  visible.value = !visible.value;
};
</script>

<style></style>
