<template>
  <q-card inline class="dorm-card q-mx-sm" :style="{ width: cardWidth, height: cardHeight }">
    <q-card-media>
      <img :src="dorm.Images[0]" style="width:100%; height:200px"/>
    </q-card-media>
    <q-card-title class="title">
    <div class="q-px-md title-content">
      <div class="ellipsis title q">{{ dorm.Name }}</div>
    </div>
  </q-card-title>
    <q-card-main>
  <div class="q-px-md main-content">
    <q-row>
      <q-col cols="12">
        <span>{{ formattedPrice }}</span>
      </q-col>
    </q-row>
    <q-row>
      <q-col cols="12">
        <p class="rating">Rating: {{ averageRating }}</p>
        <p class="address">{{ dorm.Address }}</p>
      </q-col>
    </q-row>
    <q-row>
      <q-col cols="12">
        <span><q-icon name="place" />{{ dorm.DistanceFromCampus }} km</span>
      </q-col>
    </q-row>
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
    const cardWidth = '250px'; // Set width as needed
    const cardHeight = '400px'; // Set height as needed

    return {
      averageRating,
      formattedPrice,
      cardWidth,
      cardHeight
    };
  }
});
</script>

<style scoped>
.dorm-card {
  width: 400px; /* Set width as needed */
  height: 350px; /* Set height as needed */
}

.title {
  font-size: 16px; /* Adjust font size as needed */
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

.place,
.price,
.amenities,
.average-rating,
.address {
  font-size: 14px; /* Adjust font size as needed */
  margin-bottom: 8px;
}

.average-rating,
.address {
  margin-bottom: 4px;
}


</style>
