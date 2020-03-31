<template>
  <div>
    <h2>Sorry, you are not authorised to view this page!</h2>
    <h4 class="h4margin">Get started by clicking the button to log in or sign up.</h4>
    <div>
      <b-button class="buttonsMargin" size="lg" :to="`/login`" variant="outline-dark">Log in</b-button>
      <b-button size="lg" :to="`/register`" variant="outline-dark">Register</b-button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'noAccess',
  components: {

  },
  data() {
    return {

    }
  },
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

  }
}
</script>
<style>
.h4margin{
  margin-top: 15px;
  margin-bottom: 30px;
}
</style>
