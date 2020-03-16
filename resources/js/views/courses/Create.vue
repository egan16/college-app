<template>
<b-row>
  <b-col cols="8">
    <b-card title="Add Course" tag="article">
      <b-form @submit="onSubmit">
        <b-form-group id="input-group-1" label="Title" label-form="input-1">
          <b-form-input id="input-1" type="text" required placeholder="Enter title" v-model="form.title">
          </b-form-input>

        </b-form-group>
        <b-form-group id="input-group-2" label="Code" label-form="input-2">
          <b-form-input id="input-2" type="text" required placeholder="Enter code" v-model="form.code">
          </b-form-input>
          <b-form-invalid-feedback :state"codeValid">
            The code must not be more than 5 characters long
          </b-form-invalid-feedback>
          <b-form-valid-feedback :state"codeValid">
            You got it!
          </b-form-valid-feedback>
        </b-form-group>
        <b-form-group id="input-group-3" label="Description" label-form="input-3">
          <b-form-input id="input-3" type="text" required placeholder="Enter description" v-model="form.description">
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-4" label="Points" label-form="input-4">
          <b-form-input id="input-4" type="number" required placeholder="Enter points" v-model="form.points">
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-5" label="Level" label-form="input-5">
          <b-form-input id="input-5" type="number" required placeholder="Enter level" v-model="form.level">
          </b-form-input>
        </b-form-group>

        <b-button type="submit" varient="primary">Submit
        </b-button>
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
        title: '',
        code: '',
        description: '',
        points: '',
        level: ''
      },
      loggedIn: false,
      errors: []
    }
  },
  computed: {
    codeValid() {
      return this.form.code.length <= 5 && this.form.code.length > 0
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
      this.$router.push('/');
    }
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()
      let app = this;
      let token = localStorage.getItem('token');

      axios.post('api/course', {
          title: app.form.title,
          code: app.form.code,
          description: app.form.description,
          points: app.form.points,
          level: app.form.level
        }, {
          headers: {
            Authorization: `Bearer ${token}`
          }
        })
        .then(function(response) {
          app.$router.push('/courses');
        })
        .catch(function(error) {
          console.log(error.response.data);

          app.errors = error.response.data.error
        });
    }
  }
}
</script>
<style>
</style>
