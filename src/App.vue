<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>&nbsp;|
      <span v-if="$auth.isAuthenticated">
        <!-- eslint-disable-next-line prettier/prettier -->
        <router-link to="/profile">Profile</router-link>&nbsp;|
      </span>

      <!-- Check that the SDK client is not currently loading before accessing is methods -->
      <span v-if="!$auth.loading">
        <!-- show login when not authenticated -->
        <a href="#" v-if="!$auth.isAuthenticated" @click="login">Log in</a>|
        <!-- show logout when authenticated -->
        <a href="#" v-if="$auth.isAuthenticated" @click="logout">Log out</a>|
      </span>
      <a href="#" @click="updateUserMetadata">UpdateUserMetadata</a>
    </div>
    <router-view />
  </div>
</template>

<script>
import axios from 'axios';

// Import the Auth0 configuration
import { domain } from "../auth_config.json";

export default {
  name: "App",
  methods: {
    // Log the user in
    login() {
      this.$auth.loginWithRedirect();
    },
    updateUserMetadata() {
      const userId = "";
      const accessToken = "";
      const url = "https://" + domain + "/api/v2/users/" + userId;
      const data = {"test": "test"};
      const token = "Bearer " + accessToken;
      axios.patch(
        url, 
        {user_metadata: data}, 
        {headers: { Authorization: token }}
      )
      .then(response => console.log(response.data))
      .catch(error => console.log(error));
    },
    // Log the user out
    logout() {
      this.$auth.logout({
        returnTo: window.location.origin
      });
    }
  }
};
</script>

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
