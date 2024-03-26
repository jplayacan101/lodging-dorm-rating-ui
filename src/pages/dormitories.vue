<template>
    <q-page>
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
    name: 'DormitoriesPage',
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
  