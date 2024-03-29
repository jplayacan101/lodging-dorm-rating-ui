<template>
    <div class="q-gutter-md">
      <q-row class="q-px-md row items-start q-gutter-md">
        <q-col v-for="dorm in dormitories" :key="dorm.DormID" cols="12" md="6" lg="4">
          <dorm-card :dorm="dorm" />
        </q-col>
      </q-row>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import DormCard from './DormCard.vue';
  
  export default defineComponent({
    components: {
      DormCard
    },
    props: {
      dormitories: {
        type: Array,
        required: true
      }
    }
  });
  </script>
  