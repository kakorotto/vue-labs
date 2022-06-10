<template>
  <BookStore />
</template>

<script>
// import BookApp from "./components/BookApp.vue";
import BookStore from "./components/BookStore.vue";
export default {
  name: "App",
  components: {
    // BookApp,
    BookStore,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
