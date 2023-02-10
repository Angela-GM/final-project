<template>
  <Nav />


    <h1>Edit profile</h1>
  <!-- Formulario para editar perfil  -->
  

    <form >
      <!-- imagen perfil -->
      <div>      
                <img class="profile-img"
        :src="
          avatar_url
            ? avatar_url
            : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
        "
        alt="Profile picture"
      />
      <!-- input para subir imagen avatar nueva -->

      <input type="file" name="new_avatar_url">
      <img class="icon-img" src="../icons/pen-to-square-solid.svg" alt="icon edit">
      </div>

      
     
      <!-- edit name -->
      <div>
        <label for="name">Name:</label>
        <input type="text" name="name" autocomplete="off" :value="name ? name : username" />
      </div>
      <!-- edit username -->
      <div>
        <label for="username"> Username: </label>
        <input type="text" name="username" autocomplete="off" :value="username"/>
      </div>
      <!-- edit website -->
      <div>
        <label for="website"> Website: </label>
        <input type="text" name="website" autocomplete="off" :value="website" />
      </div>

      <button>Save changes</button>
    </form>


  

</template>

<script setup>
// Imports
import { supabase } from "../supabase";
import { onMounted, ref, toRefs } from "vue";
import { useUserStore } from "../stores/user";
import Nav from "../components/Nav.vue";

// declarar variable de la UserStore
const userStore = useUserStore();

// Variables para guardar los datos del usuario
const loading = ref(false);
const username = ref(null);
const website = ref(null);
const avatar_url = ref(null);
const name = ref(null);
const image = ref(null)

// Ejecutar la funcion getProfile al cargar la página
onMounted(() => {
  getProfile();
});

// Función para traer los datos del perfil desde la store
async function getProfile() {
  await userStore.fetchUser();
  username.value = userStore.profile.username;
  avatar_url.value = userStore.profile.avatar_url;
  website.value = userStore.profile.website;
  name.value = userStore.profile.name;
  
};











</script>

<style>




</style>
