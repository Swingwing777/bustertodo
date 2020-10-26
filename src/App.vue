// src/App.vue
<template>
  <body>
    <h1>To Do List</h1>
    <header>
      <md-field>
        <md-input
          v-model="currentTodo"
          @keydown.enter="addTodo()"
          placeholder="Add a task"
        />
        <md-button
          class="md-raised md-primary"
          :disabled="!currentTodo"
          @click="addTodo()"
          >Add</md-button
        >
      </md-field>
    </header>
    <hr />

    <!-- Task entries -->

    <md-card v-if="todos.length > 0">
      <md-list class="todos md-triple-line">
        <draggable>
          <md-list-item
            v-for="todo in todos"
            :key="todo.id"
            class="md-layout md-alignment-top-between todoItem"
          >
            <!-- checkbox -->

            <input
              class="checkbox md-layout-item md-size-5"
              type="checkbox"
              :disabled="editTodoId === todo.id"
              :class="{ completed: todo.completed }"
              v-model="todo.completed"
              @change="saveTodos()"
            />

            <!-- Todo list item - alternates with editing field -->

            <span
              class="md-layout-item md-size-20"
              :class="{ completed: todo.completed }"
              @dblclick="editTodo(todo)"
              v-show="editTodoId !== todo.id"
            >
              {{ todo.label.substring(0, 40) }}
            </span>

            <!-- Task editing field -->
            <div
              class="md-layout-item md-size-20"
              v-show="editTodoId == todo.id"
            >
              <input
                type="editTask"
                v-model="todo.label"
                @keydown.enter="saveEdit()"
              />

              <button class="saveButton" @click="saveEdit()">Save</button>
            </div>

            <!-- remove task -->
            <button @click="removeTodo(todo)" class="md-layout-item md-size-10">
              <span class="md-icon md-size-1x"> delete </span>
            </button>
            <!-- <md-divider></md-divider> -->
          </md-list-item>
        </draggable>
      </md-list>
    </md-card>
  </body>
</template>

<script>
import draggable from "vuedraggable";

export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      editTodoId: null,
    };
  },

  components: {
    draggable,
  },

  mounted() {
    if (localStorage.getItem("todos")) {
      try {
        this.todos = JSON.parse(localStorage.getItem("todos"));
      } catch (e) {
        localStorage.removeItem("todos");
      }
    }
  },

  methods: {
    addTodo() {
      // to ensure a new task entry is typed
      if (!this.currentTodo) {
        return;
      }

      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false, // reactive to todo.completed checkbox
      });
      this.currentTodo = ""; // add task then clear enty field
      this.saveTodos();
    },

    saveTodos() {
      const parsed = JSON.stringify(this.todos);
      localStorage.setItem("todos", parsed);
    },

    editTodo(todo) {
      this.editTodoId = todo.id;
    },

    saveEdit() {
      this.editTodoId = null;
      this.saveTodos();
    },

    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
      this.saveTodos();
    },
  },
};
</script>

<style>
body {
  width: 90%;
  padding: 3% 0;
  max-width: 600px;
  margin: 0 auto;
  /* display: flex;
  justify-content: center; */
}

h1,
header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.saveButton {
  background: #4287f5;
  color: white;
  margin: 4px;
  padding: 3px;
  border-radius: 7%;
  border: none;
}

.todoItem {
  box-shadow: 1px 1px 2px rgb(180, 180, 180);
}

.md-layout-item {
  height: 30px;
}

.md-field {
  margin: 4px 0;
}

.md-list,
.md-triple-line,
.md-list-item-content {
  min-height: auto;
}

.editing {
  display: none;
}

.completed {
  opacity: 25%;
  text-decoration: line-through;
}
</style>
