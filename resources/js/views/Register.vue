<template>
  <div>
    <h3 v-if="loggedIn">You are logged in</h3>
    <b-form v-else @submit="onSubmit">
      <b-form-group id="input-group-1" label="Name" label-form="input-1">
        <b-form-input id="input-1" type="name" required placeholder="Enter name" v-model="form.name">
        </b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Email address" label-form="input-2" description="We'll never share your email with anyone else">
        <b-form-input id="input-2" type="email" required placeholder="Enter email" v-model="form.email">
        </b-form-input>
      </b-form-group>
      <b-form-group id="input-group-3" label="Password" label-form="input-3">
        <b-form-input id="input-3" type="password" required placeholder="Enter password" v-model="form.password">
        </b-form-input>
      </b-form-group>
      <b-button type="submit" varient="primary">Submit
      </b-button>
    </b-form>
  </div>
</template>
<script>
export default {
  name: 'register',
  components: {

  },
  data() {
    return {
      form: {
        name: '',
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

      axios.post('/api/register', {
        name: app.form.name,
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
