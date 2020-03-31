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
            <b-th>Actions</b-th>
          </b-tr>
        </b-head>
        <b-body>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{ item.name }}</b-td>
            <b-td>{{ item.address }}</b-td>
            <b-td>{{ item.phone }}</b-td>
            <b-td>{{ item.email }}</b-td>
            <b-td>
              <b-button size="sm" variant="dark" :to="`/lecturers/show/${item.id}`">View</b-button>
              <b-button size="sm" variant="outline-secondary" :to="`/lecturers/edit/${item.id}`">Edit</b-button>
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
      axios.get('/api/lecturers', {
        headers: { Authorization: 'Bearer ' + token}
      })
      .then(function (response) {
         console.log(response.data);
         app.items = response.data.data;
      })
      .catch(function (error) {
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
