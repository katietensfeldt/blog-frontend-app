<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
      |
      <router-link to="/posts">Posts</router-link>
      |
      <span v-if="isLoggedIn()">
        <router-link to="/posts/new">New Post</router-link>
        |
        <router-link to="/logout">Log Out</router-link>
      </span>
      <span v-else-if="!isLoggedIn()">
        <router-link to="/signup">Sign Up</router-link>
        |
        <router-link to="/login">Log In</router-link>
      </span>
      <br />
      <div v-if="flashMessage">
        {{ flashMessage }}
        <br />
        <button v-on:click="flashMessage = ''">Dismiss message</button>
      </div>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
export default {
  data: function () {
    return {
      flashMessage: "",
    };
  },
  created: function () {},
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
  },
};
</script>
