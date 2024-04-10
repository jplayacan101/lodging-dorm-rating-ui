<template>
    <q-page class="q-pa-md ">
      <div class="sticky-banner">
        <div class="banner q-pb-sm q-py-lg q-px-md">
            <!-- <div text-h1 text-bold class="text-h4 q-mb-sm q-py-sm">Lodging and Dorm Rating System (LDRS)</div> -->
            <q-img src="~assets/sample_img.png" alt="LDRS Banner" class="banner-image q-pa-xl" />
            <div class="filters q-pt-md">
            <div class="filter-row">
                <q-input v-model="searchQuery" label="Search" outlined dense class="search-input" />
                <q-btn class="clear-btn bg-green-3" dense color="primary" @click="clearFilters">Clear</q-btn>
            </div>
            <div class="filter-row">
                <q-input v-model="minPrice" label="Min Price" outlined dense type="number" class="filter-input" />
                <q-input v-model="maxPrice" label="Max Price" outlined dense type="number" class="filter-input q-px-md" />
                <q-input v-model="maxDistance" label="Max Distance (km)" outlined dense type="number" class="filter-input" />
            </div>
        </div>
        </div>
      </div>
  
      <q-page-container class="q-pa-md">
        
        <dorm-list :dormitories="filteredDormitories" />
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
        dormitories: [],
        filteredDormitories: [],
        searchQuery: '',
        minPrice: null,
        maxPrice: null,
        maxDistance: null
      };
    },
    async mounted() {
      await this.fetchData();
    },
    watch: {
      searchQuery() {
        this.filterDormitories();
      },
      minPrice() {
        this.filterDormitories();
      },
      maxPrice() {
        this.filterDormitories();
      },
      maxDistance() {
        this.filterDormitories();
      }
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
          this.filteredDormitories = dormsWithImages; // Initialize filtered dormitories
        } catch (error) {
          console.error('Error fetching data:', error);
        }
      },
      filterDormitories() {
        // Filter dormitories based on search query
        let filteredDormitories = this.dormitories.filter(dorm => dorm.Name.toLowerCase().includes(this.searchQuery.toLowerCase()));
  
        // Filter by price range
        if (this.minPrice !== null && this.minPrice !== undefined && this.minPrice !== "") {
          filteredDormitories = filteredDormitories.filter(dorm => dorm.Price >= this.minPrice);
        }
        if (this.maxPrice !== null && this.maxPrice !== undefined && this.maxPrice !== "") {
          filteredDormitories = filteredDormitories.filter(dorm => dorm.Price <= this.maxPrice);
        }
  
        // Filter by distance
        if (this.maxDistance !== null && this.maxDistance !== undefined && this.maxDistance !== "") {
          filteredDormitories = filteredDormitories.filter(dorm => dorm.DistanceFromCampus <= this.maxDistance);
        }
  
        this.filteredDormitories = filteredDormitories;
      },
      clearFilters() {
        this.searchQuery = '';
        this.minPrice = null;
        this.maxPrice = null;
        this.maxDistance = null;
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
    max-height: 200px; /* Adjust as needed */
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .filters {
    margin-bottom: 20px;
  }
  
  .filter-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 15px; /* Adjust spacing between rows */
  }
  
  .search-input,
  .filter-input {
    flex: 1;
  }
  
  .clear-btn {
    margin-left: 10px;
  }

  .sticky-banner {
  position: sticky;
  top: 0;
  background-color: white; /* Adjust as needed */
  z-index: 500; /* Ensure it appears above other content */
}
  </style>
  