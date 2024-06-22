<template>
    <div class="container">
      <h2>Posts</h2>
      <div class="user-select">
        <label for="userSelect">Pilih User:</label>
        <select id="userSelect" v-model="selectedUserId" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div v-if="posts.length" class="post-list">
        <div v-for="post in posts" :key="post.id" class="post-item">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </div>
      <p v-else class="no-posts">Tidak ada post yang tersedia.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUserId: null,
        posts: []
      };
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          const data = await response.json();
          this.users = data;
          if (this.users.length) {
            this.selectedUserId = this.users[0].id;
            this.fetchPosts();
          }
        } catch (error) {
          console.error('Error fetching users:', error);
        }
      },
      async fetchPosts() {
        try {
          if (this.selectedUserId) {
            const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`);
            const data = await response.json();
            this.posts = data;
          }
        } catch (error) {
          console.error('Error fetching posts:', error);
        }
      }
    },
    mounted() {
      this.fetchUsers();
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    font-family: 'Roboto', sans-serif;
  }
  
  h2 {
    font-size: 32px;
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .user-select {
    margin-bottom: 20px;
    text-align: center;
  }
  
  label {
    font-size: 18px;
    color: #555;
    margin-bottom: 10px;
    display: block;
  }
  
  select {
    width: 70%;
    padding: 12px;
    font-size: 16px;
    border: 2px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
    outline: none;
    transition: border-color 0.3s;
  }
  
  select:hover,
  select:focus {
    border-color: #3498db;
  }
  
  .post-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    list-style: none;
    padding: 0;
  }
  
  .post-item {
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .post-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }
  
  h3 {
    font-size: 22px;
    color: #333;
    margin-bottom: 10px;
  }
  
  p {
    font-size: 16px;
    color: #666;
    line-height: 1.6;
  }
  
  .no-posts {
    text-align: center;
    font-size: 18px;
    color: #999;
  }
  </style>
  