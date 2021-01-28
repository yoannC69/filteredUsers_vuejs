<template>
  <img
    alt="Vue logo"
    src="./assets/logo.png"
  >
  
  <button @click="fetchUsers">
    fetch users
  </button>
  <label>
    <input
      v-model="genderFilter"
      type="checkbox"
      value="female"
    >
    Femmes</label>
  <label>
    <input
      v-model="genderFilter"
      type="checkbox"
      value="male"
    >
    Hommes</label>

    <input type="text" v-model="search" placeholder="search users"/>

  <Users :usersFiltered="usersFiltered" />
  
        <table
      id="tbl-users"
      class="table table-hover"
    >
      <thead>
        <tr>
          <th>image</th>
          <th>Nom</th>
          <th>Email</th>
          <th>Tel</th>
          <th>Genre</th>
        </tr>
      </thead>
      <tbody id="tbody-users" v-if="users">
        <tr
          v-for="user in searchedUsers"
          :key="user.login.uuid"
        >
          <td><img :src="user.picture.thumbnail"></td>
          <td>{{ user.name.first }} {{ user.name.last }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.gender }}</td>
          <td />
        </tr>
      </tbody>
    </table>


</template>

<script>
import Users from './components/Users.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Users
  },
  data(){
    return {
      users: [],
      genderFilter: ['male','female'],
      search:''
    }
  },

  computed: {
    usersFiltered() {
      return this.users.filter((user) => this.genderFilter.includes(user.gender))
    },
    searchedUsers() {
      return this.usersFiltered.filter((user) => {
        return user.name.first.toLowerCase().match(this.search.toLowerCase());
      });
    },
  },

  methods: {
    fetchUsers() {
      axios
        .get('https://randomuser.me/api/?results=20')
        .then(response => {
          this.users = response.data.results
        })
        .catch(error => {
            console.log(error)
            this.errored = true
          })
    }
  },


}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
