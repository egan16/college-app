<template>
  <div>
    <h3 v-if="loggedIn">You are logged in.. view <router-link :to="`/courses`">courses</router-link>, view <router-link :to="`/lecturers`">lecturers</router-link></h3>
    <div v-else>
      Welcome! you can <router-link :to="`/login`">log in</router-link> or <router-link :to="`/register`">register</router-link>
    </div>
  </div>
</template>
<script>
export default {
  name: 'index',
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
