<template>
  <div>
    <h3 v-if="loggedIn">You are logged in.. view <router-link :to="`/courses`">courses</router-link></h3>
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
      form: {
        email: '',
        password: ''
      },
      loggedIn: false
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
    onSubmit(evt) {
      evt.preventDefault();

      let app = this;

      axios.post('/api/login', {
        email: app.form.email,
        password: app.form.password
      })
      .then(function(response){
        localStorage.setItem('token', response.data.token)
        app.loggedIn = true;
      })
      .catch(function(error) {
        console.log(error);
      });
    },
  }
}
</script>
<style>
</style>
