<script setup lang="ts">
import PasswordGenerator from "./components/PasswordGenerator.vue";
import ThemeSwitcher from "./components/ThemeSwitcher.vue";
</script>

<template>
  <main class="wrapper">
    <PasswordGenerator />
    <ThemeSwitcher />
  </main>
</template>

<style scoped>
.wrapper {
  position: relative;
  display: grid;
  place-content: center;
  height: 100dvh;
}
</style>
