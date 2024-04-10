<template>
  <q-layout class="bg-grey-1">
    <!-- Header content -->
    <q-header reveal elevated class="text-white bg-green-3" height-hint="61.59">
      <q-toolbar class="q-py-sm q-px-md">
        <!-- Logo or branding -->
        <q-avatar rounded size="40px">
          <img src="~assets/sizzling_sisig.png">
        </q-avatar>
        
        <!-- Navigation links -->
        <div class="row items-center no-wrap q-ml-md">
          <router-link to="/" class="text-white q-mr-sm GL__nav-link">Home</router-link>
          <router-link to="/about" class="text-white q-mr-sm GL__nav-link">About</router-link>
        </div>

        <!-- Spacer -->
        <q-space />

        <!-- Profile section or login button -->
        <div class="row items-center no-wrap q-pl-sm q-gutter-sm">
          <template v-if="isLoggedIn">
            <q-btn dense flat no-wrap>
              <q-avatar rounded size="20px">
                <img src="https://t4.ftcdn.net/jpg/03/59/58/91/360_F_359589186_JDLl8dIWoBNf1iqEkHxhUeeOulx0wOC5.jpg">
              </q-avatar>
              <q-icon name="arrow_drop_down" size="16px" />
              
              <q-menu auto-close>
                <q-list dense>
                  <q-item class="GL__menu-link-signed-in">
                    <q-item-section>
                      <div>Signed in as <strong>{{ username }}</strong></div>
                    </q-item-section>
                  </q-item>
                  <q-separator />
                  <q-item clickable class="GL__menu-link-status">
                    <q-item-section>
                      <div>
                        <q-icon name="tag_faces" color="blue-9" size="18px" />
                        Your profile
                      </div>
                    </q-item-section>
                  </q-item>
                  <q-separator />
                  <q-item clickable class="GL__menu-link-status">
                    <!-- <q-item-section>
                      <div>
                        <q-icon name="home" color="blue-9" size="18px" />
                        Add dorms
                      </div>
                    </q-item-section> -->
                    <q-item-section>
                      <router-link to="/adddorm" class="q-link">
                        <div>
                          <q-icon name="home" color="blue-9" size="18px" />
                          Add dorms
                        </div>
                      </router-link>
                    </q-item-section>
                  </q-item>
                  <q-separator />
                  <!-- <q-item clickable class="GL__menu-link">
                    <q-item-section>Your profile</q-item-section>
                  </q-item> -->
                  <!-- <q-item clickable class="GL__menu-link">
                    <q-item-section>Your dorms</q-item-section>
                  </q-item> -->
                  <!-- <q-item clickable class="GL__menu-link">
                    <q-item-section>Settings</q-item-section>
                  </q-item> -->
                  <q-item clickable class="GL__menu-link" @click="signOut">
                    <q-item-section>Sign out</q-item-section>
                  </q-item>
                </q-list>
              </q-menu>
            </q-btn>
          </template>
          <template v-else>
            <q-btn dense flat no-wrap @click="redirectToLogin">Login</q-btn>
          </template>
        </div>
      </q-toolbar>
    </q-header>
    <!-- Main content -->
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MainLayout',
  data() {
    return {
      isLoggedIn: false,
      username: '' // Initialize username
    };
  },
  created() {
    this.checkLoggedIn(); // Check login status when the component is created
  },
  watch: {
    '$route': 'checkLoggedIn' // Watch for route changes and trigger checkLoggedIn
  },
  methods: {
    checkLoggedIn() {
      const accessToken = localStorage.getItem('accessToken');
      this.isLoggedIn = !!accessToken; // Set isLoggedIn based on the presence of the access token
      // Set username if user is logged in
      if (this.isLoggedIn) {
        this.username = localStorage.getItem('userName') || '';
      }
    },
    signOut() {
      // Revoke the access token
      localStorage.removeItem('accessToken');
      // Remove username
      localStorage.removeItem('username');
      // Set isLoggedIn to false
      this.isLoggedIn = false;
      // Redirect to the login page or any other appropriate page
      this.$router.push('/login');
    },
    redirectToLogin() {
      // Redirect to the login page
      this.$router.push('/login');
    }
  }
});
</script>

<style>
.GL__nav-link {
  font-size: 16px; /* Adjust font size as needed */
  font-weight: bold; /* Adjust font weight as needed */
  text-decoration: none; /* Remove underline from links */
  margin-right: 20px; /* Adjust margin between links */
  color: #ffffff; /* Set font color */
}

.GL__nav-link:hover {
  color: #ffd700; /* Change font color on hover */
}


</style>
