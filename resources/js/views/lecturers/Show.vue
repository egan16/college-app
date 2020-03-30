<template>
  <b-row align-h="center">
    <b-col cols="12">
      <b-table-simple hover responsive>
        <b-head>
          <b-tr>
            <b-th>Name</b-th>
            <b-th>Address</b-th>
            <b-th>Phone Number</b-th>
            <b-th>Email Address</b-th>
            <!-- <b-th>Actions</b-th> -->
          </b-tr>
        </b-head>
        <b-body>
          <b-tr>
            <b-td>{{ lecturer.name }}</b-td>
            <b-td>{{ lecturer.address }}</b-td>
            <b-td>{{ lecturer.phone }}</b-td>
            <b-td>{{ lecturer.email }}</b-td>
            <!-- <b-td><router-link :to="`/lecturers/edit/${item.id}`">Edit</router-link></b-td> -->
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
      lecturer: {},
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
    axios.get(`/api/lecturers/${app.$route.params.id}`, {
      headers: { Authorization: "Bearer " + token }
    })
    .then(function (response) {
      app.lecturer = response.data.data;
    })
    .catch(function (error) {
      console.log(error);
    });

  },
  methods: {

  }
}
</script>
<style>
</style>
