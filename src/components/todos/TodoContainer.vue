<template>
  <div id="todo-cnt">
    <h1>Todo List</h1> 
    <input type="date" v-model="date" @input="fetchTodosByDate" />
    <div id="header">
      <input 
        type="text" 
        v-model="newTodo" 
        placeholder="Add a new task" 
        @keyup.enter="addTodo" 
      />
      <button 
        @click="addTodo" 
        :class="['add-btn', { 'disabled': !newTodo.trim() }]"
        :disabled="!newTodo.trim()"
      >
        +
      </button>
    </div>
    <transition-group name="fade" tag="ul">
      <li 
        v-for="(todo, index) in todos" 
        :key="index"
        class="todo-item"
      >
        <div class="list-cnt">
          <div class="todo-text">
            <input 
              type="checkbox" 
              v-model="todo.completed" 
              @click="toggleComplete(index)" 
            />
            <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
          </div>
          <button @click="removeTodo(index)" class="trash-btn">
            X
          </button>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script>

export default {
  name: 'TodoContainer',
  data() {
    return {
      newTodo: '',
      todos: [],
      date: new Date().toISOString().split('T')[0], 
    };
  },
  methods: {
    addTodo() {
      const trimmedTodo = this.newTodo.trim();
      if (trimmedTodo) {
        this.todos.push({ created_at: Date.now(), text: trimmedTodo, completed: false });
        this.newTodo = '';
        this.updateLocalStorage();
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
      this.updateLocalStorage();
    },
    toggleComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.setItem(this.date, JSON.stringify(this.todos)); 
    },
    fetchTodosByDate() {
      const todosForDate = localStorage.getItem(this.date);
      this.todos = todosForDate ? JSON.parse(todosForDate) : [];
    },
  },
  watch: {
    date() {
      this.fetchTodosByDate();
    },
  },
  mounted() {
    this.fetchTodosByDate();
  },
};
</script>

<style scoped>
@import "@/assets/styles/color.css";

#todo-cnt {
  text-align: center;
  padding: 20px;
  background-color: var(--color-primaryLight);
  border-radius: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: auto;
}

#header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}

input[type="text"] {
  width: 80%;
  padding: 8px;
  border: 2px solid var(--color-disabled);
  border-radius: 10px;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

input[type="text"]:focus {
  border-color: #00AA84;
}

.add-btn, .trash-btn {
  background-color: #00AA84;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 10px 20px;
  cursor: pointer;
  transition: transform 0.1s ease;
}

.add-btn:active, .trash-btn:active {
  transform: scale(0.95);
}

.disabled {
  background-color: var(--color-disabled);
  cursor: not-allowed;
}

ul {
  padding: 0;
  list-style-type: none;
  text-align: left;
}

.todo-item {
  padding: 8px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
 
}

.list-cnt {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.todo-text {
  display: flex;
  align-items: center;
}

input[type="checkbox"] {
  margin-right: 10px;
  cursor: pointer;

}

input[type="date"] {
  padding: 8px;
  border: 2px solid var(--color-disabled);
  border-radius: 10px;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.completed {
  text-decoration: line-through;
  color: var(--color-disabled);
}

.trash-btn {
  background-color: white;
  color: black;
  border: none;
  cursor: pointer;
  padding: 8px;
  border-radius: 6px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}


.date {
  background-color: #00AA84;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 10px 20px;
  cursor: pointer;
  transition: transform 0.1s ease;
}

.calendar-cnt {
  padding-bottom: 20px;
}

</style>

