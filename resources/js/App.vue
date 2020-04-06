<template>
  <div>
    <MyNavbar :loggedIn="this.loggedIn" v-on:login="setLoggedIn" v-on:logout="setLoggedOut" />
    <b-container>
      <router-view :loggedIn="this.loggedIn" v-on:login="setLoggedIn" v-on:logout="setLoggedOut" />
    </b-container>
  </div>
</template>
<script>
import MyNavbar from './components/MyNavbar'

export default {
  name: 'app',
  components: {
    MyNavbar
  },
  data() {
    return {
      loggedIn: false
    }
  },
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
      console.log("APP: ",this.loggedIn);
    }
    else {
      this.loggedIn = false;
    }
  },
  methods: {
    setLoggedIn() {
      this.loggedIn = true;
    },
    setLoggedOut() {
      this.loggedIn = false;
    }
  }
}
</script>
<style>
</style>
