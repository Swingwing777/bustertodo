// src/App.vue
<template>
  <div>
    <h1>To Do List</h1>
    <input
      v-model="currentTodo"
      @keydown.enter="addTodo()"
      placeholder="Add a task"
    />
    <button @click="addTodo()">Add</button>
    <hr />

    <!-- Task entries -->
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id">
        <!-- checkbox -->
        <input type="checkbox" id="checkbox" v-model="todo.completed" />

        <span
          @dblclick="editTodo(todo)"
          v-show="editTodoId !== todo.id"
          :class="{ completed: todo.completed }"
        >
          {{ todo.label }}
        </span>

        <!-- editTodo input field -->
        <input
          type="editTask"
          v-model="todo.label"
          v-show="editTodoId == todo.id"
        />
        <button @click="saveEdit()">Save</button>

        <!-- remove task -->
        <button @click="removeTodo(todo)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      editTodoId: null,
    };
  },

  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false, // reactive to todo.completed checkbox
      });
      this.currentTodo = ""; // add task then clear enty field
    },

    editTodo(todo) {
      this.editTodoId = todo.id;
    },

    saveEdit() {
      this.editTodoId = null;
    },

    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
  },
};
</script>

<style>
.editing {
  display: none;
}
</style>
