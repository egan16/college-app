<template>
<b-row align-h="center">
  <b-col cols="8">
    <h3 v-if="!loggedIn">You are not logged in!!</h3>
    <b-card v-else title="Edit Enrolment" tag="article">

      <b-form @submit="onSubmit">
        <b-form-group id="input-group-1" label="Date:" label-for="input-1">
          <b-form-input id="input-1" v-model="enrolment.date" type="date" required placeholder="Enter date"></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Time:" label-for="input-2">
          <b-form-input id="input-2" v-model="enrolment.time" type="time" required placeholder="Enter time"></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-3" label="Status:" label-for="input-3">
          <b-form-radio v-model="enrolment.status" name="status-radios" value="interested">Interested</b-form-radio>
          <b-form-radio v-model="enrolment.status" name="status-radios" value="assigned">Assigned</b-form-radio>
          <b-form-radio v-model="enrolment.status" name="status-radios" value="associate">Associate</b-form-radio>
          <b-form-radio v-model="enrolment.status" name="status-radios" value="career_break">Career break</b-form-radio>
        </b-form-group>

        <b-form-group id="input-group-4" label="Course:" label-for="input-4">
          <b-form-select name="course" v-model="enrolment.course_id">
            <option placeholder="Enter course" v-for="course in courses" v-bind:value="course.id">{{course.title}}</option>
          </b-form-select>
        </b-form-group>

        <b-form-group id="input-group-5" label="Lecturer:" label-for="input-5">
          <b-form-select name="lecturer" v-model="enrolment.lecturer_id">
            <option placeholder="Enter lecturer" v-for="lecturer in lecturers" v-bind:value="lecturer.id">{{lecturer.name}}</option>
          </b-form-select>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
      </b-form>
    </b-card>
  </b-col>
</b-row>
</template>

<script>
export default {
  data() {
    return {
      enrolment: {},
      show: true,
      loggedIn: false,
      courses: [],
      lecturers: []
    }
  },
  created() {
    // console.log(localStorage.getItem('token'));
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
    } else {
      this.loggedIn = false;
    }

    let app = this;
    let token = localStorage.getItem('token');
    axios.get(`/api/enrolments/${app.$route.params.id}`, {
        headers: {
          Authorization: "Bearer " + token
        }
      })
      .then(function(response) {
        app.enrolment = response.data.data;
      })
      .catch(function(error) {
        console.log(error);
      });

  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()

      let app = this;
      let token = localStorage.getItem('token');
      axios.put(`/api/enrolments/${app.$route.params.id}`, {
          date: app.enrolment.date,
          time: app.enrolment.time,
          status: app.enrolment.status,
          course_id: app.enrolment.course_id,
          lecturer_id: app.enrolment.lecturer_id,
        }, {
          headers: {
            Authorization: "Bearer " + token
          }
        })
        .then(function(response) {
          app.$router.push('/enrolments');
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
}
</script>
