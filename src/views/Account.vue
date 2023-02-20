<template>
  <Nav />

  <div class="profile-card">
    <!-- imagen de perfil -->
    <div>
      <img
        class="profile-img"
        :src="
          avatar_url
            ? avatar_url
            : 'https://cdn.pixabay.com/photo/2015/10/05/22/37/blank-profile-picture-973460__480.png'
        "
        alt="Profile picture"
      />
    </div>
    <!-- datos personales -->
    <div class="profile-data">
      <div>
        <img
          class="icon-img"
          src="https://img.icons8.com/windows/32/000000/username.png"
        />
        <p>{{ username }}</p>
      </div>
      <div>
        <!-- Name -->
        <img
          class="icon-img"
          src="https://img.icons8.com/windows/32/000000/name-tag-woman-horizontal.png"
        />

        <p>{{ name }}</p>
      </div>

      <div>
        <!-- Website -->
        <img
          class="icon-img"
          src="https://img.icons8.com/material-outlined/24/000000/internet.png"
        />
        <p>{{ website }}</p>
      </div>
    </div>

    <div class="center-btn">
      <router-link to="/editprofile"
        ><div class="button btn-edit-profile">Edit</div></router-link
      >
    </div>
  </div>
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
}

async function signOut() {
  try {
    loading.value = true;
    let { error } = await supabase.auth.signOut();
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}
</script>

<style>
.profile-card {
  width: 90%;
  padding: 2em 0;
  /* border: 1px solid black; */
  margin: 2em auto auto auto;
  padding: 2em 2em;
  border-radius: 20px;
  box-shadow: 0px 0px 9px 0px rgba(128, 128, 128, 0.719);
  /* background-color: #e5f4ec; */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.profile-card > div p,
.profile-card > div img {
  font-size: 0.8rem;
  margin-top: 1em;
}

.profile-card > div:nth-last-child(2) {
  margin-bottom: 2em;
}

.profile-data {
  margin-top: 2em;
}

.profile-data div {
  display: flex;
  align-items: center;
}

.btn-edit-profile {
  display: flex;
  justify-content: center;
}

.btn-edit-profile p {
  color: white;
}

.center-btn {
  width: 100%;
}
</style>
