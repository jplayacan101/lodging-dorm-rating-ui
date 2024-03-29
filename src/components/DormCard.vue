<template>
    <q-card inline class="dorm-card q-mx-sm" :style="{ width: cardWidth }">
      <q-card-media>
        <img :src="dorm.Images[0]" style="width:300px; height:200px"/>
      </q-card-media>
      <q-card-title class="title">
        <div class="q-px-md">
          <div class="ellipsis title q">{{ dorm.Name }}</div>
          <q-rating v-model="averageRating" :max="5" readonly icon-half="star_half" />
        </div>
      </q-card-title>
      <q-card-main>
        <div class="q-px-md">
          <p class="price">{{ formattedPrice }}</p>
          <q-icon name="place" /> {{ dorm.DistanceFromCampus }} km
          <p class="average-rating">Average Rating: {{ averageRating }}</p>
          <p class="address">Address: {{ dorm.Address }}</p>
          <p class="amenities text-faded">{{ dorm.Amenities }}</p>
        </div>
      </q-card-main>
      <q-card-separator />
      <q-card-actions>
        <!-- Add any additional actions here if needed -->
      </q-card-actions>
    </q-card>
  </template>
  
  <script>
  import { defineComponent, ref, computed } from 'vue';
  
  export default defineComponent({
    props: {
      dorm: {
        type: Object,
        required: true
      }
    },
    setup(props) {
      const averageRating = ref(props.dorm.AverageRating);
      const formattedPrice = computed(() => {
        // Check if Price is a number before calling toFixed()
        const price = Number(props.dorm.Price);
        return isNaN(price) ? 'N/A' : `₱${price.toFixed(2)}`;
    });
      const cardWidth = '300px'; // Set width as needed
  
      return {
        averageRating,
        formattedPrice,
        cardWidth
      };
    }
  });
  </script>
  
  <style scoped>
  .dorm-card {
    width: 350px; /* Set width as needed */
    height: 450px; 
  }
  
  .title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 8px;
  }
  
  .q-px-md {
    padding-left: 16px;
    padding-right: 16px;
  }
  
  .ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  
  .price,
  .amenities,
  .average-rating,
  .address {
    font-size: 14px;
    margin-bottom: 8px;
  }
  
  .average-rating,
  .address {
    margin-bottom: 4px;
  }
  </style>
  