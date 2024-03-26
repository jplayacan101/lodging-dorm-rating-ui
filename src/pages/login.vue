<template>
    <div class="row">
      <div class="col-lg-7 col-md-7 col-sm-7">
        <q-img src="~assets/sample_img.jpeg" style="height:800px"></q-img>
      </div>
      <div class="col-lg-5 col-md-5 col-sm-5 q-pa-sm">
        <div class="">
          <div class="text-h6 text-center text-bold q-pt-xl q-mt-xl">Login to continue</div>
          <div class="q-pa-sm">
            <div class="q-mb-md">
              <q-input v-model="email" label="Email" outlined></q-input>
            </div>
            <div class="q-mb-md">
              <q-input v-model="password" label="Password" outlined type="password"></q-input>
            </div>
          </div>
          <div class="">
            <q-checkbox v-model="check" label="Remember me"></q-checkbox>
          </div>
          <div class="q-pa-sm">
            <q-btn label="Login" style="width:100%" class="bg-green-3 text-white q-pa-sm" @click="login"></q-btn>
          </div>
          <div class="text-center q-pa-sm">
            <router-link to="/signup">No account yet? Sign up</router-link>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import axios from 'axios';
  
  export default defineComponent({
    name: 'LoginPage',
    data() {
      return {
        check: false,
        email: '',
        password: '',
      };
    },
    methods: {
      login() {
        const userData = {
          Email: this.email,
          Password: this.password,
        };
  
        axios.post('http://localhost:3000/users/login', userData)
          .then(response => {
            const accessToken = response.data.access_token;
            const username = response.data.user_name;
            localStorage.setItem('accessToken', accessToken);
            localStorage.setItem('userName', username);
            console.log("oy pota")
            console.log(response.data)
  
            // Set isLoggedIn to true and store in local storage
            // localStorage.setItem('isLoggedIn', 'true');
  
            // Redirect to the home page after successful login
            this.$router.push('/');
          })
          .catch(error => {
            console.error('Login error:', error.response.data);
          });
      },
    },
  });
  </script>
  