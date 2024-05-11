<template>
  <div class="todo-list">
    <h1>TODOLIST</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Input Data..." />
      <button type="submit" class="tambahkan">Tambahkan</button>
    </form>
    <h2>List Item</h2>
    <div class="tengah">
      <ul>
        <li v-for="(todo, index) in filteredTodos" :key="index">
          <input type="checkbox" v-model="todo.done" />
          <span v-if="!todo.editing" :class="{ 'done': todo.done }">{{ todo.text }}</span>
          <input v-else type="text" v-model="todo.text" @blur="saveEditedTodo(index)" />
          <button @click="toggleEdit(index)">{{ todo.editing ? 'Save' : 'Edit' }}</button>
          <button @click="removeTodo(index)">Remove</button>
        </li>
      </ul>
      <button @click="showActive">Tampilkan hanya yang belum selesai</button>
      <button @click="showCompleted">Tampilkan yang sudah selesai</button>
    </div>
  </div>
  <footer>
    <p>Copyright@ &copy;Aldi</p>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all', // Default filter to show all tasks
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        text: this.newTodo,
        done: false,
        editing: false // Add editing property
      });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleEdit(index) {
      this.todos[index].editing = !this.todos[index].editing;
    },
    saveEditedTodo(index) {
      this.todos[index].editing = false;
    },
    showActive() {
      this.filter = 'active'; // Filter to show only active (unfinished) tasks
    },
    showCompleted() {
      this.filter = 'completed'; // Filter to show only completed tasks
    }
  },
  computed: {
    filteredTodos() { // Use filter based on the current filter state
      switch (this.filter) {
        case 'active':
          return this.todos.filter(todo => !todo.done);
        case 'completed':
          return this.todos.filter(todo => todo.done);
        default:
          return this.todos;
      }
    },
  },
};
</script>

<style>
/* General Styles */
body {
  font-family: Arial, sans-serif;
  background-color: #191818;
  margin: 0;
  padding: 0;
}

.todo-list {
  max-width: 500px;
  margin: 50px auto;
  background-color: #fff;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

form {
  display: flex;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  outline: none;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button.tambahkan {
  background-color: #4CAF50;
  color: #fff;
}

button.tambahkan:hover {
  background-color: #45a049;
}

h2 {
  color: #333;
  font-size: 20px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

li:last-child {
  border-bottom: none;
}

input[type="checkbox"] {
  margin-right: 10px;
}

span.done {
  text-decoration: line-through;
  color: #999;
}

input[type="text"].editing {
  border: 1px solid #4CAF50;
}

button.edit {
  background-color: #2196F3;
  color: #e0a6a6;
}

button.edit:hover {
  background-color: #0b7dda;
}

button.remove {
  background-color: #f44336;
  color: #fff;
}

button.remove:hover {
  background-color: #da190b;
}

button.filter {
  margin-top: 20px;
  padding: 8px 16px;
  background-color: #2196F3;
  color: #fff;
  border: none;
  border-radius: 5px;
}

button.filter:hover {
  background-color: #0b7dda;
}
</style>

