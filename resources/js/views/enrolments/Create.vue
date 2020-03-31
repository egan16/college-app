<template>
<b-row>
  <b-col cols="8">
    <b-card title="Add Enrolment" tag="article">
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-group id="input-group-1" label="Date" label-form="input-1">
          <b-form-input id="input-1" type="date" required placeholder="Enter Date" v-model="form.date">
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-2" label="Time" label-form="input-2">
          <b-form-input id="input-2" type="time" required placeholder="Enter time" v-model="form.time">
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-3" label="Status" label-form="input-3">
          <b-form-input id="input-3" type="text" required placeholder="Enter status" v-model="form.status">
          </b-form-input>
        </b-form-group>
        <b-form-group id="input-group-4" label="Course" label-form="input-4">

          <b-form-select name="course" v-model="form.course_id">
            <option placeholder="Enter course" v-for="course in courses" v-bind:value="course.id">{{course.title}}</option>
          </b-form-select>

        </b-form-group>
        <b-form-group id="input-group-5" label="Lecturer" label-form="input-5">

          <b-form-select name="lecturer" v-model="form.lecturer_id">
            <option placeholder="Enter lecturer" v-for="lecturer in lecturers" v-bind:value="lecturer.id">{{lecturer.name}}</option>
          </b-form-select>

          <!-- <b-form-input id="input-5" type="" required placeholder="Enter lecturer" v-model="form.lecturer_id">
          </b-form-input> -->

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
        date: '',
        time: '',
        status: '',
        course_id: '',
        lecturer_id: ''
      },
      loggedIn: false,
      errors: [],
      courses: [],
      lecturers: []
    }
  },
  computed: {

  },
  // checks if user is logged in... allows user to view page if logged in
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
      let app = this;
      let token = localStorage.getItem('token');
      axios.get('/api/courses', {
          headers: {
            Authorization: 'Bearer ' + token
          }
        })
        .then(function(response) {
          console.log(response.data);
          app.courses = response.data.data;
        })
        .catch(function(error) {
          console.log(error);
        });

        axios.get('/api/lecturers', {
            headers: {
              Authorization: 'Bearer ' + token
            }
          })
          .then(function(response) {
            console.log(response.data);
            app.lecturers = response.data.data;
          })
          .catch(function(error) {
            console.log(error);
          })
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

      axios.post(`/api/enrolments`, {
          date: app.form.date,
          time: app.form.time,
          status: app.form.status,
          course_id: app.form.course_id,
          lecturer_id: app.form.lecturer_id
        }, {
          headers: {
            Authorization: `Bearer ${token}`
          }
        })
        .then(function(response) {
          app.$router.push('/enrolments');
        })
        .catch(function(error) {
          console.log(error.response.data);

          app.errors = error.response.data.error
        });
    },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.date = ''
        this.form.time = ''
        this.form.status = ''
        this.form.course_id = ''
        this.form.lecturer_id = ''
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
