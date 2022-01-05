<template>
  <ToDo />
</template>

<script>
import ToDo from "./components/ToDo.vue";
export default {
  name: "App",
  components: {
    ToDo,
  },
};
</script>
<style lang="postcss">
body {
  @apply bg-bleuby text-light;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 80px;
}
</style>
