<template>
<b-row align-h="center">
  <b-col cols="12">
    <b-table-simple hover responsive>
      <b-col cols="12">
        <b-head>
          <b-tr>
            <b-th>Title</b-th>
            <b-th>Code</b-th>
            <b-th>Description</b-th>
            <b-th>Points</b-th>
            <b-th>Level</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-head>
      </b-col>
      <b-body>
        <b-tr>
          <b-td>{{ course.title }}</b-td>
          <b-td>{{ course.code }}</b-td>
          <b-td>{{ course.description }}</b-td>
          <b-td>{{ course.points }}</b-td>
          <b-td>{{ course.level }}</b-td>
          <b-td>
            <b-button size="sm" variant="outline-secondary" :to="`/courses/edit/${course.id}`">Edit</b-button>
          </b-td>
          <b-td><b-button @delete="onDelete" type="delete" size="sm" variant="danger">Delete</b-button></b-td>
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
      course: {},
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
    axios.get(`/api/courses/${app.$route.params.id}`, {
        headers: {
          Authorization: "Bearer " + token
        }
      })
      .then(function(response) {
        app.course = response.data.data;
      })
      .catch(function(error) {
        console.log(error);
      });

  },
  methods: {
    onDelete(evt) {
      evt.preventDefault()
      let app = this;
      let token = localStorage.getItem('token');

      axios.delete("/api/courses/" + app.course.id, {
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
