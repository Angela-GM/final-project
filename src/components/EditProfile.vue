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

      <input type="file" name="new_avatar_url" @click="uploadImage">

      <img class="icon-img" src="../icons/pen-to-square-solid.svg" alt="icon edit">
      </div>


      
     
      <!-- edit name -->
      <div>
        <label for="name">Name:</label>
        <input type="text" name="name" autocomplete="off" v-model="name" />
      </div>
      <!-- edit username -->
      <div>
        <label for="username"> Username: </label>
        <input type="text" name="username" autocomplete="off" v-model="username"/>
      </div>
      <!-- edit website -->
      <div>
        <label for="website"> Website: </label>
        <input type="text" name="website" autocomplete="off" v-model="website" />
      </div>

      <button @click.prevent="editProfile">Save changes</button>
    </form>


  

</template>

<script setup>
// Imports
import { supabase } from "../supabase";
import { onMounted, ref, toRefs } from "vue";
import { useUserStore } from "../stores/user";
import Nav from "../components/Nav.vue";
import { useRouter } from "vue-router";

// declarar variable de la UserStore
const userStore = useUserStore();

// Variables para guardar los datos del usuario
const loading = ref(false);
const username = ref(null);
const website = ref(null);
const avatar_url = ref(null);
const name = ref(null);
const image = ref(null)
const redirect = useRouter();


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

// funcion para editar perfil
async function editProfile() {
  if (
    website.value.length === 0  ||
    username.value.length === 0 ||
    name.value.length === 0
  ) {
    alert("The information can not be empty");
  } else { 
   
    await userStore.fetchUser();
    await userStore.editProfile(username.value, website.value, avatar_url.value, name.value);
    redirect.push({ path: "/account" });
  }

}


//funcion para actulizar imagen de perfil
const uploadImage = async (e) => {
    const files = e.target.files;
    try {
        loading.value = true;
        if (!files || files.length === 0) {
        throw new Error("You must select an image to upload.");
        };

        const file = files[0];
        const fileExt = file.name.split(".").pop();
        const filePath = `${Math.random()}.${fileExt}`;
        console.log(filePath);

        let { error: uploadError } = await supabase.storage
        .from("avatars")
        .upload(filePath, file);

        if (uploadError) throw uploadError;
        avatar_url.value = filePath; 
        // downloadImage() => {}
        await downloadImage(filePath);
    } catch (error) {
        alert(error.message);
    } finally {
        loading.value = false;
    }
}








</script>

<style>




</style>
