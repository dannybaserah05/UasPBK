<template>
    <q-page>
      <h1 class="page-title">Album Photos</h1>
      <q-list>
        <q-item v-for="photo in photos" :key="photo.id" clickable @click="viewPhoto(photo.url)">
          <q-item-section>
            <img :src="photo.thumbnailUrl" class="photo-thumbnail" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ photo.title }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
      <q-dialog v-model="isPhotoDialogOpen">
        <img :src="currentPhotoUrl" class="dialog-photo" />
      </q-dialog>
    </q-page>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  export default {
    name: 'AlbumPhotos',
    props: {
      id: {
        type: Number,
        required: true
      }
    },
    setup(props) {
      const photos = ref([]);
      const isPhotoDialogOpen = ref(false);
      const currentPhotoUrl = ref('');
  
      const fetchPhotos = async () => {
        try {
          const response = await axios.get(`https://jsonplaceholder.typicode.com/photos?albumId=${props.id}`);
          photos.value = response.data;
        } catch (error) {
          console.error('Error fetching photos:', error);
        }
      };
  
      onMounted(fetchPhotos);
  
      const viewPhoto = (url) => {
        currentPhotoUrl.value = url;
        isPhotoDialogOpen.value = true;
      };
  
      return {
        photos,
        isPhotoDialogOpen,
        currentPhotoUrl,
        viewPhoto
      };
    }
  };
  </script>
  
  <style scoped>
  .page-title {
    font-size: 32px;
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .photo-thumbnail {
    max-width: 100px;
    max-height: 100px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .photo-thumbnail:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }
  
  .dialog-photo {
    max-width: 100%;
    border-radius: 8px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  @media (max-width: 768px) {
    .photo-thumbnail {
      max-width: 80px;
      max-height: 80px;
    }
  }
  </style>
  