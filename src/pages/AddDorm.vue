<template>
  <q-page>
    <q-page-container class="q-pt-md q-px-md">
      <q-card class="q-pa-md">
        <q-card-section class="text-h6">Add Dormitory</q-card-section>
        <q-card-section>
          <q-form @submit="submitForm">
            <q-input class="q-pa-sm" v-model="dorm.Name" label="Name" outlined dense />
            <q-input class="q-pa-sm" v-model="dorm.Address" label="Address" outlined dense />
            <q-input class="q-pa-sm" v-model="dorm.Price" label="Price" outlined type="number" dense />
            <q-input class="q-pa-sm" v-model="dorm.Amenities" label="Amenities" outlined dense />
            <q-input class="q-pa-sm" v-model="dorm.Latitude" label="Latitude" outlined dense />
            <q-input class="q-pa-sm" v-model="dorm.Longitude" label="Longitude" outlined dense />
            <div class="q-mt-md">
              <q-rating v-model="dorm.AverageRating" label="Average Rating" :max="5" color="warning" dense />
            </div>
            <input type="file" @change="handleImageChange" accept="image/*" class="q-mt-md" />

            <q-btn type="submit" label="Add Dormitory" color="primary" :disable="!isFormValid" />
          </q-form>
        </q-card-section>
      </q-card>
    </q-page-container>

    <!-- Success notification modal -->
    <q-dialog v-model="showSuccessNotification" @before-hide="closeSuccessDialog">
      <q-card class="popup-card">
        <q-card-section class="q-pa-md">
          <q-icon name="check" class="text-green text-h4"></q-icon>
          <q-space />
          <div class="text-h6">Dormitory added successfully!</div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn color="positive" label="Close" @click="closeSuccessDialog" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <!-- Error notification modal -->
    <q-dialog v-model="showErrorNotification" @before-hide="closeErrorDialog">
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
  </q-page>
</template>

<script>
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
  name: 'AddDorm',
  setup() {
    const dorm = ref({
      Name: '',
      Address: '',
      Price: null,
      Amenities: '',
      Latitude: null,
      Longitude: null,
      AverageRating: 0,
      ImageURL: null
    });

    const showSuccessNotification = ref(false);
    const showErrorNotification = ref(false);
    const errorMessage = ref('');
    const router = useRouter();

    const handleImageChange = (event) => {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        dorm.value.ImageURL = e.target.result;
      };
      reader.readAsDataURL(file);
    };

    const isFormValid = computed(() => {
      return (
        dorm.value.Name !== '' &&
        dorm.value.Address !== '' &&
        dorm.value.Price !== null &&
        dorm.value.Amenities !== '' &&
        dorm.value.Latitude !== null &&
        dorm.value.Longitude !== null
      );
    });

    const submitForm = async () => {
      try {
        const formData = {
          Name: dorm.value.Name,
          Address: dorm.value.Address,
          Price: dorm.value.Price,
          Amenities: dorm.value.Amenities,
          Latitude: dorm.value.Latitude,
          Longitude: dorm.value.Longitude,
          AverageRating: dorm.value.AverageRating,
          ImageURL: dorm.value.ImageURL,
          UserID: localStorage.userID
        };

        const response = await axios.post('http://localhost:3000/dormitories', formData, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log('Dormitory added successfully:', response.data);
        showSuccessNotification.value = true;
        setTimeout(() => {
          closeSuccessDialog();
        }, 5000);
      } catch (error) {
        console.error('Error adding dormitory:', error.response.data);
        errorMessage.value = error.response.data.message || 'An error occurred. Please try again later.';
        showErrorNotification.value = true;
      }
    };

    const closeSuccessDialog = () => {
      showSuccessNotification.value = false;
      router.push('/');
    };

    const closeErrorDialog = () => {
      showErrorNotification.value = false;
    };

    return {
      dorm,
      handleImageChange,
      submitForm,
      showSuccessNotification,
      showErrorNotification,
      errorMessage,
      closeSuccessDialog,
      closeErrorDialog,
      isFormValid
    };
  }
};
</script>

<style scoped>
.q-input__control,
.q-rating,
input[type="file"] {
  border-radius: 0.5rem;
}

.q-rating__icon {
  font-size: 1.5rem;
}

.q-btn {
  border-radius: 0.5rem;
}

.q-card__section {
  padding: 0.5rem;
}

.q-form {
  padding: 0.5rem;
}

.q-mt-md {
  margin-top: 1.5rem;
}
</style>
