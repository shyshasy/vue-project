<template>
  <div id="app">
    <div class="container">
      <h1>Ma To-Do List 🎉</h1>
      <div class="input-container">
        <input 
          v-model="newTodoText" 
          @keyup.enter="addTodo" 
          placeholder="Ajouter une tâche"
          class="todo-input"
        />
        <input 
          v-model="newTodoDate" 
          type="date"
          class="date-input"
        />
        <select v-model="newTodoStatus" class="status-select">
          <option value="Incomplet">Incomplet</option>
          <option value="Complété">Complété</option>
        </select>
        <button @click="addTodo" class="add-button">Ajouter</button>
      </div>
      <ul class="todo-list">
        <li v-for="todo in todos" :key="todo.id" class="todo-item">
          <input 
            type="checkbox" 
            v-model="todo.completed" 
            class="todo-checkbox"
          />
          <span :class="{ 'completed': todo.completed }" class="todo-text">
            {{ todo.text }}
          </span>
          <span class="todo-date">{{ todo.date }}</span>
          <span :class="{ 'status-completed': todo.completed, 'status-incomplete': !todo.completed }" class="todo-status">
            {{ todo.status }}
          </span>
          <button @click="editTodo(todo.id)" class="edit-button">✎</button>
          <button @click="removeTodo(todo.id)" class="remove-button">✖</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      newTodoText: '',
      newTodoDate: '',
      newTodoStatus: 'Incomplet',
      todos: [],
      editingTodo: null
    };
  },
  methods: {
    addTodo() {
      if (this.newTodoText.trim()) {
        if (this.editingTodo !== null) {
          const todo = this.todos.find(todo => todo.id === this.editingTodo);
          todo.text = this.newTodoText;
          todo.date = this.newTodoDate;
          todo.status = this.newTodoStatus;
          this.editingTodo = null;
        } else {
          this.todos.push({
            id: Date.now(),
            text: this.newTodoText,
            date: this.newTodoDate,
            status: this.newTodoStatus,
            completed: this.newTodoStatus === 'Complété'
          });
        }
        this.newTodoText = '';
        this.newTodoDate = '';
        this.newTodoStatus = 'Incomplet';
      }
    },
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    editTodo(id) {
      const todo = this.todos.find(todo => todo.id === id);
      this.newTodoText = todo.text;
      this.newTodoDate = todo.date;
      this.newTodoStatus = todo.status;
      this.editingTodo = id;
    }
  }
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f7f9fc;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid #ff5722;
}

h1 {
  text-align: center;
  color: #ff5722;
}

.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input, .date-input, .status-select {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.status-select {
  flex: 1;
}

.add-button {
  padding: 10px 20px;
  border: none;
  background-color: #4caf50;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #388e3c;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
  background-color: #f9f9f9;
  border-left: 5px solid #ff5722;
  transition: background-color 0.3s;
}

.todo-item:hover {
  background-color: #e0e0e0;
}

.todo-checkbox {
  margin-right: 10px;
}

.todo-text {
  flex: 1;
  font-size: 16px;
}

.completed {
  text-decoration: line-through;
  color: #6c757d;
}

.todo-date {
  font-size: 14px;
  color: #9e9e9e;
  margin-right: 10px;
}

.todo-status {
  font-size: 14px;
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 4px;
  margin-right: 10px;
}

.status-completed {
  background-color: #4caf50;
  color: white;
}

.status-incomplete {
  background-color: #ff9800;
  color: white;
}

.edit-button, .remove-button {
  background: none;
  border: none;
  cursor: pointer;
  transition: color 0.3s;
}

.edit-button {
  color: #2196f3;
  font-size: 18px;
  margin-right: 10px;
}

.edit-button:hover {
  color: #1976d2;
}

.remove-button {
  color: #f44336;
  font-size: 18px;
}

.remove-button:hover {
  color: #d32f2f;
}
</style>
