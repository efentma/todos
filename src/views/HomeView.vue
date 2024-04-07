<template>
  <div class="container">
    <h2 class="title">TODO LIST</h2>
    <div class="add-btn">
      <nav>
        <button @click="addData">Tambah Data</button>
      </nav>
      <router-view/>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>ID</th>
          <th>Title</th>
          <th>Completed</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in paginatedTodos" :key="index">
          <td>{{ todo.id }}</td>
          <td>{{ todo.title }}</td>
          <td>{{ todo.completed }}</td>
          <td class="table-btn">
            <button @click="editTodo(todo.id)" class="edit-btn">Edit</button>
            <button @click="deleteTodo(todo.id)" class="delete-btn">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  
    <div class="pagination">
      <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      todos: [],
      pageSize: 10,
      currentPage: 1
    };
  },
  created() {
    this.fetchData();
  },
  computed: {
    paginatedTodos() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.todos.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.todos.length / this.pageSize);
    }
  },
  methods: {
    fetchData() {
      axios
        .get("https://jsonplaceholder.typicode.com/todos")
        .then((response) => {
          this.todos = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    addData() {
      this.$router.push('/add')
    },
    editTodo(id) {
      // Implement your edit logic here, for example, redirect to edit page
      console.log('Edit todo with ID:', id);
    },
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => {
          this.todos = this.todos.filter(todo => todo.id !== id);
        })
        .catch(error => {
          console.log(error);
        });
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    }
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.title {
  font-size: 36px;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}
.add-btn {
  width: 80%;
  display: flex;
  justify-content: flex-start;
  margin-bottom: 10px;
}
.add-button {
  margin-bottom: 20px;
}

.table {
  width: 80%;
  margin-top: 20px;
  border-collapse: collapse;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow: hidden;
}
.table th,
.table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #f0f0f0;
}

.table th {
  background-color: #f8f8f8;
  color: #333;
}

.table tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

.table tbody tr:hover {
  background-color: #f0f0f0;
}

.edit-btn {
  cursor: pointer;
  padding: 6px 12px;
  margin-right: 3px;
  border: none;
  border-radius: 4px;
  background-color: #28a745; /* Green */
  color: #fff;
}

.edit-btn:hover {
  background-color: #218838;
}

.delete-btn {
  cursor: pointer;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  background-color: #dc3545; /* Red */
  color: #fff;
}

.delete-btn:hover {
  background-color: #c82333;
}

.pagination {
  display: flex;
  justify-content: space-between;
  width: 80%; /* Sesuaikan lebar dengan lebar tabel */
  margin-top: 20px;
}

.pagination button {
  cursor: pointer;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: #fff;
}

.pagination button:hover {
  background-color: #0056b3;
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
