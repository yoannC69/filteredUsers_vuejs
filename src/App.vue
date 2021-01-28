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

  <Users :usersFiltered="usersFiltered" />
  
        


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
      genderFilter: ['male','female']
    }
  },

  computed: {
    usersFiltered() {
      return this.users.filter((user) => this.genderFilter.includes(user.gender))
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
