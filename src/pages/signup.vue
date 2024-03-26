<template>
    <div class="row">
      <div class="col-lg-7 col-md-7 col-sm-7">
        <!-- Placeholder for any signup-related image -->
        <q-img src="~assets/sample_img.jpeg" style="height:800px"></q-img>
      </div>
      <div class="col-lg-5 col-md-5 col-sm-5 q-pa-sm">
        <div class="">
          <div class="text-h6 text-center text-bold q-pt-xl q-mt-xl">Sign Up for an Account</div>
          <div class="q-mb-md">
            <q-input v-model="username" label="Username" outlined></q-input>
          </div>
          <div class="q-mb-md">
            <q-input v-model="email" label="Email" outlined></q-input>
          </div>
          <div class="q-mb-md">
            <q-input v-model="password" label="Password" outlined type="password"></q-input>
          </div>
          <div class="q-mb-md">
            <q-input v-model="confirmPassword" label="Confirm Password" outlined type="password"></q-input>
          </div>
          <div class="q-mb-md">
            <q-input v-model="phone" label="Phone" outlined></q-input>
          </div>
          <div class="q-mb-md">
            <q-input v-model="socialMedia" label="Social Media" outlined></q-input>
          </div>
          <div class="q-pa-sm">
            <q-btn label="Sign Up" style="width:100%" class="bg-green-3 text-white q-pa-sm" @click="signUp"></q-btn>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import axios from 'axios';
  
  export default defineComponent({
    name: 'SignupPage',
    data() {
      return {
        username: '',
        email: '',
        password: '',
        confirmPassword: '',
        phone: '',
        socialMedia: '',
      };
    },
    methods: {
      signUp() {
        if (this.password !== this.confirmPassword) {
          // Handle password mismatch error
          console.error('Passwords do not match');
          return;
        }
  
        // Formulate CreateUserDto object to be sent to the API
        const createUserDto = {
          Username: this.username,
          Email: this.email,
          Password: this.password,
          Phone: this.phone,
          SocialMedia: this.socialMedia,
        };
  
        // Make POST request to signup API endpoint
        axios.post('http://localhost:3000/users', createUserDto)
          .then(response => {
            // Handle successful signup (e.g., show success message, redirect to login page)
            console.log('Signup successful:', response.data);
            // Redirect to login page after successful signup
            this.$router.push('/login');
          })
          .catch(error => {
            // Handle signup error (e.g., display error message)
            console.error('Signup error:', error.response.data);
          });
  
        // Reset form fields after submission if needed
        this.username = '';
        this.email = '';
        this.password = '';
        this.confirmPassword = '';
        this.phone = '';
        this.socialMedia = '';
      },
    },
  });
  </script>
  