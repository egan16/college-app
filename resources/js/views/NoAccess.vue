<template>
  <div>
    <h3>Sorry, you are not authorised to view this page! you can <router-link :to="`/login`">log in</router-link> or <router-link :to="`/register`">register</router-link></h3>
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
</style>
