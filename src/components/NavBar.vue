<template>
  <nav id="nav">
    <router-link to="/" id="logo"><img :src="logo" :alt="alt" /></router-link>
    <router-link to="/">Home</router-link> |
    <router-link to="/pedidos">Pedidos</router-link>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
  props: ["logo", "alt"],
};
</script>

<style scoped>
#nav {
  background: #222;
  border-bottom: 2px solid #111;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

#nav #logo {
  margin: auto;
  margin-left: 0;
}

#logo img {
  width: 40px;
  height: 40px;
}

#nav a {
  color: #fcba03;
  text-decoration: none;
  margin: 12px;
  transition: 0.3s;
}

#nav a:hover {
  color: #fff;
}
</style>
