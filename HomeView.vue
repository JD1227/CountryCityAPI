<template>
  <div class="home">
    <nav>Welcome!</nav>

    <HelloWorld />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
};
</script>

<style scoped>
nav {
  padding: 30px;
  font-size: 25px;
}
</style>
