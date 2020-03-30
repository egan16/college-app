<template>
  <b-row align-h="center">
    <b-col cols="12">
      <b-table-simple hover responsive>
        <b-head>
          <b-tr>
            <b-th>name</b-th>
            <b-th>email</b-th>
          </b-tr>
        </b-head>
        <b-body>
          <b-tr>
            <b-td>{{ user.name }}</b-td>
            <b-td>{{ user.email }}</b-td>
            <!-- <b-td><router-link :to="`/courses/edit/${item.id}`">Edit</router-link></b-td> -->
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
      user: {},
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
    axios.get(`/api/user/${app.$route.params.id}`, {
      headers: { Authorization: "Bearer " + token }
    })
    .then(function (response) {
      app.user = response.data.data;
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
