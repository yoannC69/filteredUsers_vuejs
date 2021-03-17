<template>
  <h1>Modification profil</h1>
  <form class="container-form">
    <img
      class="avatarProfil"
      :src="user.avatarUrl"
      alt="avatarProfil"
    >
    <div class="form-group">
      <label for="firstname">Prénom</label>
      <input
        id="firstname"
        v-model="user.firstName"
        type="text"
        class="form-control"
        placeholder="entrez votre prénom"
      >
    </div>
    <div class="form-group">
      <label for="Lastname">Nom</label>
      <input
        id="Lastname"
        v-model="user.lastName"
        type="text"
        class="form-control"
        placeholder="entrez votre nom"
      >
    </div>
    <div class="form-group">
      <label for="exampleInputEmail1">Email</label>
      <input
        id="exampleInputEmail1"
        v-model="user.email"
        type="email"
        class="form-control"
        placeholder="Entrez votre email"
      >
    </div>
    <div class="form-group">
      <label for="birthdate">date de naissance</label>
      <input
        id="birthdate"
        v-model="user.birthDate"
        type="date"
        class="form-control"
      >
    </div>
    
    <p>Choisissez votre genre</p>

    <div>
      <input
        id="femme"
        v-model="user.gender"
        type="radio"
        name="genre"
        value="female"
      >
      <label for="femme">femme</label>
    </div>

    <div>
      <input
        id="homme"
        v-model="user.gender"
        type="radio"
        name="genre"
        value="male"
      >
      <label for="homme">homme</label>
    </div>
    
    <button
      type="submit"
      class="btn btn-primary"
      @click="editUser"
    >
      Enregistrer
    </button>
    <button
      type="submit"
      class="btn btn-primary"
      @click="deleteUser"
    >
      supprimer l'utilisteur
    </button>
  </form>
</template>
<script>
    import axios from "axios"

    export default ({

        name: 'EditUser',

        component: {},

        data() {
            return {
                user: {}
            }
        },
        created(){this.fetchUser();},

        methods:{
            fetchUser(){
            axios
            .get(`http://localhost:6929/users/${this.$route.params.id}`)
            .then(response => this.user = response.data)
            },
            async editUser(){
              //console.log(this.user, 'a')
              const response = await axios
              .put(`http://localhost:6929/users/${this.$route.params.id}`, this.user)
              this.message = response.data.messageError
              if(!this.message){
                this.$toast.success(`Vous avez modifié l'utilisateur`),
                response => this.user = response.data
              } else{
                this.$toast.error(this.message)
              }
            },
            deleteUser(){
              axios
              .delete(`http://localhost:6929/users/${this.$route.params.id}`, this.user)
              .then(this.$toast.success(`Vous avez supprimé l'utilisateur`),
                this.$router.push({ path : '/users' }));
            }
        }
    })
</script>
<style>
.container-form{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 40px;
}
h1{
  margin-bottom: 40px;
}
.form-group{
  width: 20%;
}
.avatarProfil{
  height: 280px;
}
</style>
