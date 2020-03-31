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
        <b-tr v-for="item in items" :key="item.id">
          <b-td>{{ item.date }}</b-td>
          <b-td>{{ item.time }}</b-td>
          <b-td>{{ item.status }}</b-td>
          <b-td>
            <router-link :to="`/courses/show/${item.course.id}`">{{ item.course.title }}</router-link>
          </b-td>
          <b-td>
            <router-link :to="`/lecturers/show/${item.lecturer.id}`">{{ item.lecturer.name }}</router-link>
          </b-td>
          <b-td>
            <b-button size="sm" variant="dark" :to="`/enrolments/show/${item.id}`">View</b-button>
            <b-button size="sm" variant="outline-secondary" :to="`/enrolments/edit/${item.id}`">Edit</b-button>
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
      items: []
    }
  },
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
      let app = this;
      let token = localStorage.getItem('token');
      axios.get('/api/enrolments', {
          headers: {
            Authorization: 'Bearer ' + token
          }
        })
        .then(function(response) {
          console.log(response.data);
          app.items = response.data.data;
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

  }
}
</script>
<style>
</style>
