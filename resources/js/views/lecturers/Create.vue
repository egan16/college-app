<template>
<b-row>
  <b-col cols="8">
    <b-card title="Add Lecturer" tag="article">
      <b-form @submit="onSubmit" @reset="onReset">

              <b-form-group id="input-group-1" label="Name" label-form="input-1">
                <b-form-input id="input-1" type="text" required placeholder="Enter name" v-model="form.name">
                </b-form-input>
              </b-form-group>

              <b-form-group id="input-group-2" label="Address" label-form="input-2">
                <b-form-input id="input-2" type="text" required placeholder="Enter address" v-model="form.address">
                </b-form-input>
              </b-form-group>

              <b-form-group id="input-group-3" label="Phone Number" label-form="input-3">
                <b-form-input id="input-3" type="text" required placeholder="Enter phone number" v-model="form.phone">
                </b-form-input>
              </b-form-group>

              <b-form-group id="input-group-4" label="Email Address" label-form="input-4">
                <b-form-input id="input-4" type="email" required placeholder="Enter email address" v-model="form.email">
                </b-form-input>
              </b-form-group>

              <b-button type="submit" varient="primary">Submit</b-button>
              <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </b-card>
  </b-col>
</b-row>
</template>
<script>
export default {
  data() {
    return {
      form: {
        name: '',
        address: '',
        phone: '',
        email: ''
      },
      loggedIn: false,
      errors: []
    }
  },
  computed: {

  },
  // checks if user is logged in... allows user to view page if logged in
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
    }
    // if not logged in refirected to index page
    else {
      this.loggedIn = false;
      this.$router.push('/noAccess');
    }
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()
      let app = this;
      let token = localStorage.getItem('token');

      axios.post(`/api/lecturers`, {
          name: app.form.name,
          address: app.form.address,
          phone: app.form.phone,
          email: app.form.email
        }, {
          headers: {
            Authorization: `Bearer ${token}`
          }
        })
        .then(function(response) {
          app.$router.push('/lecturers');
        })
        .catch(function(error) {
          console.log(error.response.data);

          app.errors = error.response.data.error
        });
    },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.name = ''
        this.form.address = ''
        this.form.phone = ''
        this.form.email = ''
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
  }
}
</script>
<style>
</style>
