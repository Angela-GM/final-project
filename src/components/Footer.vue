<template>
  <footer>
    <ul>
      <li>
        <a href="https://www.linkedin.com/in/angelagarciam82/" target="_black"
          ><i class="fa-brands fa-linkedin"></i
        ></a>
      </li>
      <li>
        <a href="https://github.com/Angela-GM" target="_blank"
          ><i class="fa-brands fa-github"></i
        ></a>
      </li>
      <li>
        <a href="#"><i class="fa-brands fa-square-instagram"></i></a>
      </li>
      <li>
        <a href="#"><i class="fa-brands fa-square-twitter"></i></a>
      </li>
      <li>
        <a href="mailto:angelagm0112@gmail.com" target="_blank"
          ><i class="fa-solid fa-envelope"></i
        ></a>
      </li>
    </ul>

    <small
      ><i class="fa-regular fa-copyright"></i> {{ currentYear }} Angela Garcia
      Muñoz | FrontEnd Developer |
      <a href="https://github.com/Angela-GM/final-project" target="_blank"
        >Final Project Ironhack</a
      >
    </small>
  </footer>
</template>

<script setup>
import { ref } from "vue";
const currentYear = ref(new Date().getFullYear());
</script>

<style></style>
