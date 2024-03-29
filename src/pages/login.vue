<template>
  <div class="row">
    <div class="col-lg-7 col-md-7 col-sm-7">
      <q-img src="~assets/sample_img.jpeg" style="height:800px"></q-img>
    </div>
    <div class="col-lg-5 col-md-5 col-sm-5 q-pa-sm">
      <div class="">
        <div class="text-h6 text-center text-bold q-pt-xl q-mt-xl q-mb-md">Login to continue</div>
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

  <!-- Popup notification for errors -->
  <q-dialog v-model="showErrorNotification" @before-hide="clearError" ref="errorDialog">
    <q-card class="popup-card">
      <q-card-section class="q-pa-md">
        <q-icon name="warning" class="text-red text-h4"></q-icon>
        <q-space />
        <div class="text-h6">{{ errorMessage }}</div>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn color="negative" label="Close" @click="closeErrorDialog" />
      </q-card-actions>
    </q-card>
  </q-dialog>
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
      errorMessage: '',
      showErrorNotification: false,
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
          console.log("Login successful");
          console.log(response.data);

          // Redirect to the home page after successful login
          this.$router.push('/');
        })
        .catch(error => {
          if (error.response && error.response.status === 401) {
            // Unauthorized: Invalid email or password
            this.errorMessage = 'Invalid email or password';
            this.showErrorNotification = true; // Show notification
            setTimeout(() => {
              this.showErrorNotification = false; // Hide notification after 5 seconds
            }, 5000);
          } else {
            // Other errors
            console.error('Login error:', error.response.data);
          }
        });
    },
    clearError() {
      // Clear error message and hide notification
      this.errorMessage = '';
      this.showErrorNotification = false;
    },
    closeErrorDialog() {
      this.$refs.errorDialog.hide();
    },
  },
});
</script>

<style>
.popup-card {
  max-width: 300px;
}
</style>
