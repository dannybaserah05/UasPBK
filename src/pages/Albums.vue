<template>
    <q-page>
      <h1 class="page-title">Albums</h1>
      <q-list separator class="album-list">
        <q-item v-for="album in albums" :key="album.id" clickable @click="goToAlbum(album.id)">
          <q-item-section>
            <div class="album-item">
              <div class="album-title">{{ album.title }}</div>
              <div class="album-info">
                <q-icon name="fas fa-images" size="24px" class="album-icon"></q-icon>
                <span>{{ album.photos.length }} Photos</span>
              </div>
            </div>
          </q-item-section>
        </q-item>
      </q-list>
    </q-page>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Albums',
    setup() {
      const albums = ref([]);
      const router = useRouter();
  
      const fetchAlbums = async () => {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/albums');
          albums.value = response.data.map(album => ({ ...album, photos: [] })); // Placeholder for photos
        } catch (error) {
          console.error('Error fetching albums:', error);
        }
      };
  
      const goToAlbum = (id) => {
        router.push({ name: 'AlbumPhotos', params: { id } });
      };
  
      onMounted(fetchAlbums);
  
      return {
        albums,
        goToAlbum,
      };
    },
  };
  </script>
  
  <style scoped>
  .page-title {
    font-size: 32px;
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .album-list {
    max-width: 600px;
    margin: 0 auto;
  }
  
  .album-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .album-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }
  
  .album-title {
    font-size: 18px;
    color: #333;
    font-weight: bold;
  }
  
  .album-info {
    display: flex;
    align-items: center;
  }
  
  .album-icon {
    color: #3498db;
    margin-right: 10px;
  }
  
  @media (max-width: 768px) {
    .album-item {
      flex-direction: column;
      align-items: flex-start;
    }
  
    .album-info {
      margin-top: 10px;
    }
  }
  </style>
  