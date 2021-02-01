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

  <input  
    v-model="search"
    type="text"
    placeholder="search users"
  >

  <Users :users-filtered="usersFiltered" />
  
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
        <th>
          Age
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            @click="sortLowest"
          >
            <path d="M3 3a1 1 0 000 2h11a1 1 0 100-2H3zM3 7a1 1 0 000 2h5a1 1 0 000-2H3zM3 11a1 1 0 100 2h4a1 1 0 100-2H3zM13 16a1 1 0 102 0v-5.586l1.293 1.293a1 1 0 001.414-1.414l-3-3a1 1 0 00-1.414 0l-3 3a1 1 0 101.414 1.414L13 10.414V16z" />
          </svg>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            @click="noOrder"
          >
            <path
              fill-rule="evenodd"
              d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
              clip-rule="evenodd"
            />
          </svg>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            @click="sortHighest"
          >   
            <path d="M3 3a1 1 0 000 2h11a1 1 0 100-2H3zM3 7a1 1 0 000 2h7a1 1 0 100-2H3zM3 11a1 1 0 100 2h4a1 1 0 100-2H3zM15 8a1 1 0 10-2 0v5.586l-1.293-1.293a1 1 0 00-1.414 1.414l3 3a1 1 0 001.414 0l3-3a1 1 0 00-1.414-1.414L15 13.586V8z" />
          </svg>
        </th>
      </tr>
    </thead>
    <tbody
      v-if="users"
      id="tbody-users"
    >
      <tr
        v-for="user in searchedUsers" 
        :key="user.login.uuid"
      >
        <td><img :src="user.picture.thumbnail"></td>
        <td>{{ user.name.first }} {{ user.name.last }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.phone }}</td>
        <td>{{ user.gender }}</td>
        <td>{{ user.dob.age }}</td>
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
          this.users.push.apply(this.users,response.data.results)
        })
        .catch(error => {
            console.log(error)
            this.errored = true
          })
    },
    sortLowest() {
      this.users.sort((a, b) => a.dob.age > b.dob.age ? 1 : -1);
    },
    sortHighest() {
      this.users.sort((a, b) => a.dob.age < b.dob.age ? 1 : -1);
    },
    noOrder() {
      this.users.sort((a, b) => a.dob.age == b.dob.age ? 1 : -1);
    },
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
