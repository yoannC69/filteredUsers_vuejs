<template>
  <h1>Modification profil</h1>
  <div
    class="container-form"
  >
    <div class="form-group">
      <label for="avatarUrl">Url photo</label>
      <input
        id="avatarUrl"
        v-model="user.avatarUrl"
        type="text"
        class="form-control"
        placeholder="entrez l'url de votre photo"
      >
    </div>
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
      type=""
      class="btn btn-primary"
      @click="createUser"
    >
      Enregistrer
    </button>
  </div>
</template>

<script>
    import axios from "axios"

    export default ({

        name: 'CreateUser',

        component: {},

        data() {
            return {
                user: {}, message:''
            }
        },

        methods:{
            async createUser(){
                console.log(this.user, 'a')
                const response = await axios
                .post('http://localhost:6929/users', this.user)
                this.message = response.data.messageError
                if(!this.message){
                  this.$toast.success(`Vous avez créer l'utilisateur`),
                  this.$router.push({ path : '/users' });
                } else{
                  this.$toast.error(this.message)
                }
                
            }
        }
    })
</script>