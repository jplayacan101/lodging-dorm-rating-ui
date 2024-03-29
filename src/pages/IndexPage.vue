<template>
  <q-page class="q-pa-md">
    <div class="banner q-pb-md">
      <div text-h1 text-bold class="text-h4 q-mb-sm q-py-sm">Lodging and Dorm Rating System (LDRS)</div>
      <q-img src="~assets/sample_img.jpeg" alt="LDRS Banner" class="banner-image" />
    </div>

    <q-page-container class="q-pa-md">
      <dorm-list :dormitories="dormitories" />
    </q-page-container>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import axios from 'axios';
import DormList from '../components/DormList.vue';

export default defineComponent({
  name: 'HomePage',
  components: {
    DormList
  },
  data() {
    return {
      dormitories: []
    };
  },
  async mounted() {
    await this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        // Fetch dormitories
        const dormitoriesResponse = await axios.get('http://localhost:3000/dormitories');
        const dormitories = dormitoriesResponse.data;

        // Fetch dormitory images
        const dormitoryImagesResponse = await axios.get('http://localhost:3000/dormitory-images');
        const dormitoryImages = dormitoryImagesResponse.data;

        // Match dormitory images with dormitories based on DormID
        const dormsWithImages = dormitories.map(dorm => {
          const images = dormitoryImages.filter(image => image.DormID === dorm.DormID).map(image => image.ImageURL);
          return { ...dorm, Images: images.length > 0 ? images : ['https://www.ateneo.edu/sites/default/files/inline-images/imagesmith-20190206-2-42-HDR.jpg'] };
        });

        this.dormitories = dormsWithImages;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
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


