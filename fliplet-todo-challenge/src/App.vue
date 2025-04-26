<template>
  <div id="app">
    <img alt="Fliplet" src="./assets/logo.png" class="logo" />
    <TodoComponent />
  </div>
</template>

<script>
import TodoComponent from "./components/TodoComponent.vue";

export default {
  name: "App",
  components: {
    TodoComponent,
  },
};
</script>

<style>
body {
  background: #20262e;
  padding: 20px;
  font-family: Helvetica;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  transition: all 0.2s;
}
.logo {
  width: 150px;
}
</style>
