<template>
<b-row align-h="center">
  <b-col cols="12">
    <b-table-simple hover responsive>
      <b-col cols="12">
        <b-head>
          <b-tr>
            <b-th>Name</b-th>
            <b-th>Address</b-th>
            <b-th>Phone Number</b-th>
            <b-th>Email Address</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-head>
      </b-col>
      <b-body>
        <b-tr>
          <b-td>{{ lecturer.name }}</b-td>
          <b-td>{{ lecturer.address }}</b-td>
          <b-td>{{ lecturer.phone }}</b-td>
          <b-td>{{ lecturer.email }}</b-td>
          <b-td>
            <b-button size="sm" variant="outline-secondary" :to="`/lecturers/edit/${lecturer.id}`">Edit</b-button>
          </b-td>
        </b-tr>
      </b-body>
    </b-table-simple>
    <!-- <b-button @delete="onDelete" type="delete" variant="danger">Delete</b-button> -->
  </b-col>
</b-row>
</template>
<script>
export default {
  data() {
    return {
      lecturer: {},
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
    axios.get(`/api/lecturers/${app.$route.params.id}`, {
        headers: {
          Authorization: "Bearer " + token
        }
      })
      .then(function(response) {
        app.lecturer = response.data.data;
      })
      .catch(function(error) {
        console.log(error);
      });

  },
  methods: {
    // onDelete(evt) {
    //   evt.preventDefault()
    //   let app = this;
    //   let token = localStorage.getItem('token');
    //
    //   axios.delete(`/api/lecturers/${app.$route.params.id}`, {
    //       name: app.form.name,
    //       address: app.form.address,
    //       phone: app.form.phone,
    //       email: app.form.email
    //     }, {
    //       headers: {
    //         Authorization: `Bearer ${token}`
    //       }
    //     })
    //     .then(function(response) {
    //       app.$router.push('/lecturers');
    //     })
    //     .catch(function(error) {
    //       console.log(error.response.data);
    //
    //       app.errors = error.response.data.error
    //     });
    // }
  }
}
</script>
<style>
</style>
