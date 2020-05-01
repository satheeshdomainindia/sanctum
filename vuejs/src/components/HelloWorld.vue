<template>
  <div class="hello">
   welcome <button v-if="!loggedIn" @click="login" >Login </button>
   <div v-else>
     <button @click="getUser">Get User</button>
     <button @click="logout">Logout</button>
   </div>
  </div>
</template>

<script>
import axios from 'axios'

axios.defaults.withCredentials = true;

export default {
  data(){
    return{
      loggedIn : localStorage.getItem('loggedIn') == 'true'
    }

  },
  methods: {
    login(){
          axios.get('http://localhost:8000/sanctum/csrf-cookie').then(response => {
          console.log(response);

            axios.post('http://localhost:8000/api/login',{
                  email: "admin@app.com",
                  password : "password"
                }).then(res => {
                  console.log(res.data);
                  localStorage.setItem('loggedIn','true');
                  this.loggedIn = true;
                  })
        });
     
    },

    logout(){
        axios.post('http://localhost:8000/api/logout').then(() => {                 
                  this.loggedIn = false;
                  localStorage.removeItem('loggedIn');
                  });

    },

    getUser(){

      axios.get('http://localhost:8000/api/user').then(res => {
                  console.log(res.data);
                  });


    }


  },
}
</script>
