<template>
  <div class="container">
    <div class="todo-header">
      <h1>TODO LIST</h1>
    </div>
    <div class="todo-form">
      <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Masukin Tugas" />
      <button @click="addTodo">Add</button>
    </div>
    <ul class="todo-list">
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <input type="checkbox" :checked="todo.completed" @change="toggleTodo(index)" />
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <button @click="removeTodo(index)">Delete</button>
      </li>
    </ul>
    <p>Tugas belum selesai: {{ unfinishedTodosCount }}</p>
  </div>
</template>

<script>
import { useTodoStore } from '../stores/todoStore';
import { computed, ref } from 'vue';

export default {
  setup() {
    const todoStore = useTodoStore();
    const newTodo = ref('');

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value);
        newTodo.value = '';
      }
    };

    const removeTodo = (index) => {
      todoStore.removeTodo(index);
    };

    const toggleTodo = (index) => {
      todoStore.toggleTodo(index);
    };

    const todos = computed(() => todoStore.todos);
    const unfinishedTodosCount = computed(() => todoStore.unfinishedTodosCount);

    return {
      newTodo,
      addTodo,
      removeTodo,
      toggleTodo,
      todos,
      unfinishedTodosCount,
    };
  },
};
</script>

<style>
body {
  background-image: url(https://wallpapercave.com/wp/3IjkTnr.jpg);
  background-repeat: no-repeat;
  background-size: cover;
}
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #0a2d50;
  border: 2px solid #ced4da;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
 color: #000000;
}

.todo-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.todo-header h1 {
  font-size: 24px;
  color: #ffffff;
  font-family: High Jakarta;
}

.todo-form {
  display: flex;
  align-items: center;
  margin-bottom: 10px; /* Margin bottom for spacing */
}

.todo-form input[type="text"] {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ced4da;
  border-radius: 4px;
}

.todo-form button {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.todo-form button:hover {
  background-color: #0056b3;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 4px;
  transition: transform 0.3s ease;
}

.todo-item:hover {
  transform: translateY(-3px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

.todo-item .completed {
  text-decoration: line-through;
  color: #6c757d;
}

.todo-item button {
  margin-left: 10px;
  padding: 6px 12px;
  font-size: 14px;
  color: #fff;
  background-color: #dc3545;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.todo-item button:hover {
  background-color: #c82333;
}
</style>
