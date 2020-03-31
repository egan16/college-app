<template>
  <div>
    <div v-if="loggedIn">
      <h2>Welcome to the college app!</h2>
      <h4 class="h4margin">Get started by clicking the button of the section you want to visit.</h4>
      <div>
        <b-button class="buttonsMargin" size="lg" :to="`/courses`" variant="outline-dark">View courses</b-button>
        <b-button class="buttonsMargin" size="lg" :to="`/lecturers`" variant="outline-dark">View lecturers</b-button>
        <b-button size="lg" :to="`/enrolments`" variant="outline-dark">View enrolments</b-button>
      </div>
    </div>
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
.h4margin{
  margin-top: 15px;
  margin-bottom: 30px;
}
.buttonsMargin{
  margin-right: 15px;
}
</style>
