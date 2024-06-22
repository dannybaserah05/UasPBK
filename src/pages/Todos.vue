<template>
    <div class="todo-container">
      <h2>Daftar Kegiatan</h2>
      <div class="todo-list">
        <div v-for="kegiatan in kegiatanList" :key="kegiatan.id" :style="{ backgroundColor: kegiatan.color }" class="todo-item">
          <div class="todo-info">
            <input type="checkbox" v-model="kegiatan.isDone" class="checkbox" />
            <span :class="{ 'done': kegiatan.isDone }">{{ kegiatan.nama }}</span>
          </div>
          <button @click="deleteKegiatan(kegiatan)" class="delete-button">
            <i class="fas fa-trash-alt"></i>
          </button>
        </div>
      </div>
      <form @submit.prevent="addKegiatan" class="add-form">
        <input type="text" v-model="newKegiatan.nama" placeholder="Tambah Kegiatan Baru" class="input-text" />
        <button type="submit" class="add-button">
          <i class="fas fa-plus"></i> Tambah
        </button>
      </form>
      <button @click="showCompletedTasks" class="show-completed-button">
        <i class="fas fa-check"></i> Tampilkan Kegiatan yang Selesai
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        kegiatanList: [
          { id: 1, nama: 'Bersih-bersih rumah', isDone: false, color: '#f9e79f' },
          { id: 2, nama: 'Beli bahan makanan', isDone: false, color: '#a2d9ce' },
          { id: 3, nama: 'Kumpul bersama teman', isDone: false, color: '#d5dbdb' }
        ],
        newKegiatan: { nama: '' }
      };
    },
    methods: {
      addKegiatan() {
        if (this.newKegiatan.nama.trim()) {
          const colors = ['#f9e79f', '#a2d9ce', '#d5dbdb'];
          const randomColor = colors[Math.floor(Math.random() * colors.length)];
          this.kegiatanList.push({ id: this.kegiatanList.length + 1, nama: this.newKegiatan.nama, isDone: false, color: randomColor });
          this.newKegiatan.nama = '';
        }
      },
      deleteKegiatan(kegiatan) {
        this.kegiatanList = this.kegiatanList.filter(k => k.id !== kegiatan.id);
      },
      showCompletedTasks() {
        this.kegiatanList = this.kegiatanList.filter(kegiatan => kegiatan.isDone);
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 600px;
    margin: 40px auto;
    padding: 20px;
    background-color: #000000;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    font-family: 'Arial', sans-serif;
  }
  
  h2 {
    text-align: center;
    color: #3498db;
    font-size: 28px;
    margin-bottom: 20px;
  }
  
  .todo-list {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 15px;
  }
  
  .todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .todo-item:hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }
  
  .todo-info {
    display: flex;
    align-items: center;
  }
  
  .checkbox {
    margin-right: 15px;
    width: 20px;
    height: 20px;
    border-radius: 4px;
    cursor: pointer;
    appearance: none;
    background-color: #ff0000;
    border: none;
    transition: background-color 0.3s;
  }
  
  .checkbox:checked {
    background-color: #ff0101;
  }
  
  span {
    font-size: 18px;
    color: #333;
  }
  
  .done {
    text-decoration: line-through;
    color: #7f8c8d;
  }
  
  .delete-button {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 18px;
    color: #e74c3c;
    transition: color 0.3s;
  }
  
  .delete-button:hover {
    color: #c0392b;
  }
  
  .add-form {
    display: flex;
    align-items: center;
    margin-top: 20px;
  }
  
  .input-text {
    flex: 1;
    padding: 12px;
    border: 2px solid #bdc3c7;
    border-radius: 5px;
    margin-right: 10px;
    font-size: 16px;
  }
  
  .add-button {
    padding: 12px 20px;
    background-color: #3498db;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 16px;
  }
  
  .add-button:hover {
    background-color: #2980b9;
  }
  
  .show-completed-button {
    margin-top: 20px;
    padding: 12px 20px;
    background-color: #ff0000;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    display: block;
    width: 100%;
    text-align: center;
    transition: background-color 0.3s;
    font-size: 16px;
  }
  
  .show-completed-button:hover {
    background-color: #27ae60;
  }
  </style>
  