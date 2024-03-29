<template>
  <q-page class="q-pa-md">
    <div class="banner q-pb-md">
      <div text-h1 text-bold class="text-h4 q-mb-sm q-py-sm">Lodging and Dorm Rating System (LDRS)</div>
      <q-img src="~assets/sample_img.jpeg" alt="LDRS Banner" class="banner-image" />
    </div>

    <div class="content q-mb-lg text-center">
      <p class="text-body1 q-mb-md">
        The LDRS aims to provide a service wherein the public may efficiently and systematically search for and review lodging and dormitory facilities within the Katipunan, Barangka, and Pansol areas via a user rating system.
      </p>
      <p class="text-body1 q-mb-md">
        It is intended to allow users, especially those potentially interested in living spaces in these areas for their proximity to Ateneo de Manila University, easy access to a central database providing them with peer-reviewed information on the quality of available accommodations and housing—aiding them in their choice of lodging.
      </p>
      <p class="text-body1 q-mb-md">
        The LDRS includes the creation of a database of accommodations within these locations, the implementation of searching, rating, reviewing, and filtering functionalities, the design of a user-friendly interface for browsing and submitting reviews, and the integration of administrative tools for managing user accounts and moderating content.
      </p>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'AboutPage',
  methods: {
    exploreNow() {
      // Add navigation logic if needed
    }
  }
});
</script>

<style scoped>
.banner {
  text-align: center;
}

.banner-image {
  max-width: 100%;
  height: auto;
  max-height: 300px; /* Adjust as needed */
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.content {
  background-color: #f7f7f7;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.explore-btn {
  width: 200px;
  margin: 0 auto;
  display: block;
}
</style>
