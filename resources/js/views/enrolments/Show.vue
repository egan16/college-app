<template>
<b-row align-h="center">
  <b-col cols="12">
    <b-table-simple hover responsive>
      <b-col cols="12">
        <b-head>
          <b-tr>
            <b-th>Date</b-th>
            <b-th>Time</b-th>
            <b-th>Status</b-th>
            <b-th>Course</b-th>
            <b-th>Lecturer</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-head>
      </b-col>
      <b-body>
        <b-tr>
          <b-td>{{ enrolment.date }}</b-td>
          <b-td>{{ enrolment.time }}</b-td>
          <b-td>{{ enrolment.status }}</b-td>
          <b-td>
            <router-link :to="`/courses/show/${enrolment.course.id}`">{{ enrolment.course.title }}</router-link>
          </b-td>
          <b-td>
            <router-link :to="`/lecturers/show/${enrolment.lecturer.id}`">{{ enrolment.lecturer.name }}</router-link>
          </b-td>
          <b-td>
            <b-button size="sm" variant="outline-secondary" :to="`/enrolments/edit/${enrolment.id}`">Edit</b-button>
          </b-td>
        </b-tr>
      </b-body>
    </b-table-simple>
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

  }
}
</script>
<style>
</style>
