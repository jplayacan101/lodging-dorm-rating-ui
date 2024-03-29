<template>
  <div class="row">
    <div class="col-lg-7 col-md-7 col-sm-7">
      <!-- Placeholder for any signup-related image -->
      <q-img src="~assets/sample_img.jpeg" style="height:800px"></q-img>
    </div>
    <div class="col-lg-5 col-md-5 col-sm-5 q-pa-sm">
      <div class="">
        <div class="text-h6 text-center text-bold q-pt-xl q-mt-xl q-mb-md">Sign Up for an Account</div>
        <div class="q-mx-md">
          <q-input v-model="username" label="Username" outlined :rules="usernameRules"></q-input>
        </div>
        <div class="q-mx-md">
          <q-input v-model="email" label="Email" outlined :rules="emailRules"></q-input>
        </div>
        <div class="q-mx-md">
          <q-input v-model="password" label="Password" outlined type="password" :rules="passwordRules"></q-input>
        </div>
        <div class="q-mx-md">
          <q-input v-model="confirmPassword" label="Confirm Password" outlined type="password" :rules="confirmPasswordRules"></q-input>
        </div>
        <div class="q-mx-md">
          <q-input v-model="phone" label="Phone" outlined :rules="phoneRules"></q-input>
        </div>
        <div class="q-mx-md">
          <q-input v-model="socialMedia" label="Social Media" outlined></q-input>
        </div>
        <div class="q-pa-sm">
          <q-btn label="Sign Up" style="width:100%" class="bg-green-3 text-white q-pa-sm" @click="signUp" :disable="!isSignUpValid"></q-btn>
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

  <!-- Success notification modal -->
  <q-dialog v-model="showSuccessNotification" @before-hide="clearSuccess" ref="successDialog">
    <q-card class="popup-card">
      <q-card-section class="q-pa-md">
        <q-icon name="check" class="text-green text-h4"></q-icon>
        <q-space />
        <div class="text-h6">Signup successful! You can now log in.</div>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn color="positive" label="Close" @click="closeSuccessDialog" />
      </q-card-actions>
    </q-card>
  </q-dialog>
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
      showErrorNotification: false,
      showSuccessNotification: false,
      errorMessage: '',
      usernameRules: [
        (v) => !!v || 'Username is required',
      ],
      emailRules: [
        (v) => !!v || 'Email is required',
        (v) => /.+@.+\..+/.test(v) || 'Email must be valid',
      ],
      passwordRules: [
        (v) => !!v || 'Password is required',
        (v) => v && v.length >= 3 || 'Password must be at least 3 characters',
        (v) => v && /(?=.*[a-zA-Z])(?=.*[0-9])/.test(v) || 'Password must contain at least one letter and one number',
      ],
      confirmPasswordRules: [
        (v) => !!v || 'Confirm Password is required',
        (v) => v === this.password || 'Passwords do not match',
      ],
      phoneRules: [
        (v) => !v || /^\d{11}$/.test(v) || 'Phone must be 11 digits',
      ],
    };
  },
  computed: {
    isSignUpValid() {
      return (
        this.usernameValid &&
        this.emailValid &&
        this.passwordValid &&
        this.confirmPasswordValid &&
        this.phoneValid
      );
    },
    usernameValid() {
      return !!this.username;
    },
    emailValid() {
      return !!this.email && /.+@.+\..+/.test(this.email);
    },
    passwordValid() {
      return !!this.password && this.password.length >= 3 && /(?=.*[a-zA-Z])(?=.*[0-9])/.test(this.password);
    },
    confirmPasswordValid() {
      return !!this.confirmPassword && this.confirmPassword === this.password;
    },
    phoneValid() {
      return !this.phone || /^\d{11}$/.test(this.phone);
    },
  },
  methods: {
    signUp() {
      if (!this.usernameValid) {
        this.errorMessage = 'Username is required';
      } else if (!this.emailValid) {
        this.errorMessage = 'Email must be valid';
      } else if (!this.passwordValid) {
        this.errorMessage = 'Password must be at least 3 characters and contain at least one letter and one number';
      } else if (!this.confirmPasswordValid) {
        this.errorMessage = 'Passwords do not match';
      } else if (!this.phoneValid) {
        this.errorMessage = 'Phone must be 11 digits';
      } else {
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
            // Display success modal
            this.showSuccessNotification = true;
            setTimeout(() => {
              // Redirect to login page after 5 seconds
              this.showSuccessNotification = false;
              this.$router.push('/login');
            }, 5000);
          })
          .catch(error => {
            // Handle signup error (e.g., display error message)
            this.errorMessage = error.response.data.message || 'Signup failed. Please try again later.';
            this.showErrorNotification = true;
            console.error('Signup error:', error.response.data);
            this.setAutoCloseErrorNotification(5000); // Close error notification after 5 seconds
          });
      }
    },
    clearError() {
      // Clear error message and hide notification
      this.errorMessage = '';
      this.showErrorNotification = false;
    },
    closeErrorDialog() {
      this.$refs.errorDialog.hide();
    },
    clearSuccess() {
      // Clear success notification
      this.showSuccessNotification = false;
    },
    closeSuccessDialog() {
      // Close success dialog
      this.$refs.successDialog.hide();
    },
    setAutoCloseErrorNotification(duration) {
      // Auto-close error notification after a duration
      setTimeout(() => {
        this.showErrorNotification = false;
      }, duration);
    },
  },
});
</script>

<style>
.popup-card {
  max-width: 300px;
}
</style>
