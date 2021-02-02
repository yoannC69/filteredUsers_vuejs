<template>
  <div class="headt">
    <button
      class="btn btn-primary"
      @click="fetchUsers"
    >
      Réupérer des utilisateurs
    </button>
    <label>
      <input
        v-model="genderFilter"
        type="checkbox"
        value="male"
      >
      Hommes
    </label>
    <label>
      <input
        v-model="genderFilter"
        type="checkbox"
        value="female"
      >
      Femmes
    </label>
    <label>
      Rechercher :
      <input
        v-model="search"
        type="text"
        placeholder="Rechercher"
      >
    </label>  
    <label>Trier par âge :          
    </label>
    <p v-if="sortDirection === ''">
      Par défaut
    </p>
    <p v-if="sortDirection === 'asc'">
      Croissant
    </p>
    <p v-if="sortDirection === 'desc'">
      Décroissant
    </p>
  </div>
  <p v-if="users.length">
    il y a <strong>{{ searchedUsers.length }}</strong> utilisateurs
  </p>
  <p v-else>
    il n'y a <strong>aucun</strong> utilisateur
  </p>
  <table
    v-if="users.length"
    class="table table-hover"
  >
    <thead>
      <tr>
        <th>Photo</th>
        <th>Nom</th>
        <th>Email</th>
        <th>Tel</th>
        <th>Genre</th>
        <th>
          <button
            class="btn btn-light"
            @click="changeSort"
          >
            Âge
            <i
              v-if="sortDirection"
              class="fa"
              :class="[ sortDirection === 'asc' ? 'fa-sort-up' : 'fa-sort-down' ]"
            />
          </button>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="user in searchedUsers"
        :key="user.email"
      >
        <td><img :src="user.picture.thumbnail"></td>
        <td>{{ user.name.first }} {{ user.name.last }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.phone }}</td>
        <td>{{ user.gender }}</td>
        <td>{{ user.dob.age }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      users: [],
      errored: false,
      genderFilter: ['male', 'female'],
      search: '',
      sortDirection: ''
    }
  },
  computed: {
    searchedUsers() {
      return this.users
      .filter((user) => this.genderFilter.includes(user.gender))
      .filter((user) => {
        return (user.name.first.toLowerCase().includes(this.search.toLowerCase())) ||
        (user.name.last.toLowerCase().includes(this.search.toLowerCase()))
      })
      .sort((a,b) => {
        if (!this.sortDirection) return 0;
        const  modifier = this.sortDirection === 'desc' ? -1 : 1;
        return (a.dob.age - b.dob.age) * modifier;
      })
    },
  },

  //created(){ this.fetchUsers()},
  
  methods: {
    fetchUsers() { 
      axios
        .get('https://randomuser.me/api/?results=20')
        .then(response => {
         this.users = [...this.users, ...response.data.results]
         //this.users = this.users.concat(response.data.results)
        })
        .catch(error => {
          console.error(error)
          this.errored = true
        })
    },
    changeSort() {
      if (this.sortDirection === ''){
        this.sortDirection = 'asc'
        return this.users
      }else if (this.sortDirection === 'asc'){
        this.sortDirection = 'desc'
      } else if (this.sortDirection === 'desc'){
        this.sortDirection = ''
      }
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
.headt{
  display: flex;
  justify-content: space-around;
}
.btn-primary{
  background-color: #41B883!important;
  border-color: #41B883!important;
}
.btn-primary:hover{
  background-color: #35495E!important;
}
</style>
