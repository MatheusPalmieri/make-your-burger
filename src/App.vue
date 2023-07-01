<template>
  <NavBar :logo="logo" :alt="alt" />

  <router-view />

  <Footer />
</template>

<script>
import NavBar from "./components/NavBar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    NavBar,
    Footer,
  },
  data() {
    return {
      logo: "/img/logo.png",
      alt: "Make Your Burger",
    };
  },
};
</script>

<style>
* {
  font-family: Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.container {
  margin: 50px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font: 42px;
  color: #222;
  margin-bottom: 30px;
}
</style>
