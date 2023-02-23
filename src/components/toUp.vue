<template>
  <button id="up" @click="scrollToTop">
    <i class="fa-solid fa-circle-up"></i>
  </button>
</template>

<script setup>
const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
};
</script>

<style></style>
