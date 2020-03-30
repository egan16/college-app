<template>
<div>
  <b-navbar toggleable="sm" type="dark" variant="dark">
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <!-- <router-link to="/">Home</router-link> -->
        <b-nav-item to="/">Home</b-nav-item>
        <b-nav-item-dropdown text="Courses" left>
          <b-dropdown-item to="/courses">View All</b-dropdown-item>
          <b-dropdown-item to="/courses/create">Create</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <div v-if="loggedIn">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template v-slot:button-content>
              <em>User</em>
            </template>
            <!-- View user by id -->
            <router-link :to="`/users/1`">Profile</router-link>
            <b-dropdown-item @click="logout()">Log Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </div>
        <div v-else>
          <b-nav-item to="/login">Login</b-nav-item>
          <b-nav-item to="/register">Register</b-nav-item>
        </div>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>
<script>
export default {
  name: "myNavbar",
  // checks if user is logged in... allows user to view page if logged in
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
    }
    // if not logged in refirected to index page
    else {
      this.loggedIn = false;
    }
  },
  methods: {
    logout() {
      localStorage.removeItem('token');
      console.log("Logged out");
      // this.$router.push('/');
    }
  }
};
</script>
<style>
.navbar {
  margin-bottom: 40px;
}
</style>
