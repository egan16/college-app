<template>
<b-row align-h="center">
  <b-col cols="12">
    <b-table-simple hover responsive  id="my-table"

      :per-page="perPage"
      :current-page="currentPage"
      small>
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
      <b-body>
        <b-tr v-for="item in items" :key="item.id">
          <b-td>
            <router-link :to="`/courses/show/${item.id}`">{{ item.title }}</router-link>
          </b-td>
          <b-td>{{ item.code }}</b-td>
          <b-td>{{ item.description }}</b-td>
          <b-td>{{ item.points }}</b-td>
          <b-td>{{ item.level }}</b-td>
          <b-td>
            <router-link :to="`/courses/edit/${item.id}`">Edit</router-link>
          </b-td>
        </b-tr>
      </b-body>
    </b-table-simple>

    <b-pagination v-model="currentPage" :total-rows="rows" :per-page="perPage" aria-controls="my-table"></b-pagination>

    <p class="mt-3">Current Page: {{ currentPage }}</p>

  </b-col>
</b-row>
</template>
<script>
export default {
  data() {
    return {
      perPage: 6,
      currentPage: 1,
      items: []
    }
  },
  computed: {
      rows() {
        return this.items.length
      }
    },
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
