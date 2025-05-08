<script setup>
import { ref, onMounted } from "vue";

const theme = ref("light");

const toggleTheme = () => {
  theme.value = theme.value === "light" ? "dark" : "light";
  document.documentElement.style.colorScheme = theme.value;
};

onMounted(() => {
  document.documentElement.style.colorScheme = theme.value;
});
</script>

<template>
  <div class="container">
    <h1>Password Generator</h1>

    <p>This is {{ theme }} mode.</p>

    <button @click="toggleTheme">
      Switch to {{ theme === "light" ? "Dark" : "Light" }} Mode
    </button>
  </div>
</template>

<style scoped>
.container {
  padding: 2rem;
  border-radius: 8px;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background-color: light-dark(#2563eb, #3b82f6);
  color: white;
  cursor: pointer;
}

button:hover {
  opacity: 0.9;
}
</style>
