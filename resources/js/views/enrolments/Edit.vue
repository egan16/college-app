<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <b-card
        v-else
        title="Edit Enrolment"
        tag="article"
      >

        <b-form @submit="onSubmit">
          <b-form-group
            id="input-group-1"
            label="Date:"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="enrolment.date"
              type="date"
              required
              placeholder="Enter date"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Time:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="enrolment.time"
              type="time"
              required
              placeholder="Enter time"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-3"
            label="Status:"
            label-for="input-3"
          >
            <b-form-input
              id="input-3"
              v-model="enrolment.status"
              type="text"
              required
              placeholder="Enter status"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-4"
            label="Course:"
            label-for="input-4"
          >
            <b-form-input
              id="input-4"
              v-model="enrolment.course.title"
              type=""
              required
              placeholder="Enter course"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-5"
            label="Lecturer:"
            label-for="input-5"
          >
            <b-form-input
              id="input-5"
              v-model="enrolment.lecturer.name"
              type=""
              required
              placeholder="Enter lecturer_id"
            ></b-form-input>
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
        loggedIn: false
      }
    },
    created() {
      // console.log(localStorage.getItem('token'));
      if (localStorage.getItem('token')) {
        this.loggedIn = true;
      }
      else {
        this.loggedIn = false;
      }

      let app = this;
      let token = localStorage.getItem('token');
      axios.get(`/api/enrolments/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.enrolment = response.data.data;
      })
      .catch(function (error) {
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
        },
        {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response) {
          app.$router.push('/enrolments');
        })
        .catch(function (error) {
          console.log(error);
        });
      }
    }
  }
</script>
