<template>
  <div class="add-todo-container">
    <h1 class="title">Tambah Data</h1>
    <form @submit.prevent="addTodo" class="add-form">
      <div class="form-group">
        <label for="title">UserId:</label>
        <input type="text" v-model="newTodo.id" id="id" placeholder="Masukkan ID">
      </div>
      <div class="form-group">
        <label for="title">Judul:</label>
        <input type="text" v-model="newTodo.title" id="title" placeholder="Masukkan judul">
      </div>
      <div class="form-group">
        <label for="completed">Selesai:</label>
        <select v-model="newTodo.completed" id="completed">
          <option value="true">Selesai</option>
          <option value="false">Belum Selesai</option>
        </select>
      </div>
      <button type="submit">Simpan</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      newTodo: {
        userId: '',
        title: '',
        completed: false
      }
    };
  },
  methods: {
    addTodo() {
      axios.post('https://jsonplaceholder.typicode.com/todos/', this.newTodo)
        .then(response => {
          // Data berhasil ditambahkan

          this.$emit('todo-added', response.data); // Emit event untuk memberi tahu komponen induk bahwa todo telah ditambahkan
          this.$router.push('/'); // Redirect ke halaman utama
        })
        .catch(error => {
          console.error(error);
        });
    }
  }
}
</script>

<style scoped>
.add-todo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.title {
  font-size: 36px;
  color: #333;
  margin-bottom: 20px;
}

.add-form {
  width: 300px;
}

.form-group {
  margin-bottom: 10px;
}

.form-group label {
  display: block;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 5px;
  font-size: 16px;
}

button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
