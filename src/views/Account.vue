<template>
  <Nav />

  <!-- Name -->
  <h1>Name: {{username}}</h1>
  <!-- Edit name -->
<div>

</div>



  <h2>Website: {{ website }}</h2>



  <img :src="avatar_url ? avatar_url : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'" alt="Profile picture">
  




</template>

<script setup>
// Imports
  import { supabase } from '../supabase'
  import { onMounted, ref, toRefs } from 'vue'
  import { useUserStore } from "../stores/user";
  import Nav from '../components/Nav.vue';

// declarar variable de la UserStore
  const userStore = useUserStore();

  // Variables para guardar los datos del usuario
  const loading = ref(false);
  const username = ref(null);
  const website = ref(null);
  const avatar_url = ref(null);


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
  }

  async function signOut() {
    try {
      loading.value = true
      let { error } = await supabase.auth.signOut()
      if (error) throw error
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }
</script>

<style>
img {
  width: 200px;
  border-radius: 50%;
}
</style>