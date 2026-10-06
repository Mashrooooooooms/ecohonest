<script setup>
import Main from './components/Main.vue'
</script>

<template>
  <Main />
</template>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

body {
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

input,
button,
textarea,
select {
  font: inherit;
  color: inherit;
}

button {
  background: none;
  border: none;
  cursor: pointer;
}

a {
  color: inherit;
  text-decoration: none;
}

ul,
ol {
  list-style: none;
}
</style>