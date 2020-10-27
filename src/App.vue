// src/App.vue
<template>
  <body>
    <div class="container md-elevation-3">
      <div>
        <h1>To Do List</h1>
        <md-card-header class="md-layout">
          <md-field>
            <md-input
              class="md-layout-item"
              v-model="currentTodo"
              @keydown.enter="addTodo()"
              placeholder="Add a task"
            />
            <md-button
              class="addButton md-raised md-primary md-layout-item md-size-20"
              :disabled="!currentTodo"
              @click="addTodo()"
              >Add</md-button
            >
          </md-field>
        </md-card-header>

        <!-- Task entries -->

        <md-list v-if="todos.length > 0" class="todos md-double-line">
          <draggable>
            <md-list-item
              v-for="todo in todos"
              :key="todo.id"
              class="md-layout todoItem"
            >
              <!-- checkbox -->

              <input
                class="md-layout-item md-alignment-center-left md-size-10"
                type="checkbox"
                :disabled="editTodoId === todo.id"
                :class="{ completed: todo.completed }"
                v-model="todo.completed"
                @change="saveTodos()"
              />

              <!-- Todo list item - alternates with editing field -->

              <span
                class="md-layout-item md-alignment-center-left"
                :class="{ completed: todo.completed }"
                @dblclick="editTodo(todo)"
                :disabled="todo.completed"
                v-show="editTodoId !== todo.id"
              >
                {{ todo.label.substring(0, 40) }}
              </span>

              <!-- Task editing field -->
              <div
                class="md-layout-item md-alignment-center-left"
                v-show="editTodoId == todo.id"
              >
                <input
                  type="editTask"
                  :disabled="todo.completed"
                  v-model="todo.label"
                  @keydown.enter="saveEdit()"
                />

                <button class="saveButton" @click="saveEdit()">Save</button>
              </div>

              <!-- remove task -->
              <button
                @click="removeTodo(todo)"
                class="md-layout-item md-alignment-center-right md-size-10"
              >
                <span class="md-icon md-size-1x"> delete </span>
              </button>
              <md-divider></md-divider>
            </md-list-item>
          </draggable>
        </md-list>
      </div>
    </div>
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
  width: 100%;
}

.container {
  width: auto;
  padding: 2%;
  margin-top: 1%;
  background: #8498b6;
}

div {
  width: 100%;
  padding: 3% 0;
  max-width: 600px;
  margin: 0 auto;
}

h1,
header {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  text-shadow: 0 1px 1px rgb(218, 218, 218);
}

.taskField {
  font-size: large;
}

.taskInput {
  margin-left: 10px;
  color: black;
  text-shadow: 0 1px 1px white;
  font-size: x-large;
}

.addButton {
  border: 1px solid black;
}

.saveButton {
  background: #4287f5;

  color: white;
  margin: 4px;
  padding: 3px;
  border-radius: 7%;
  border: none;
}
.md-layout-item {
  height: auto;
}

.todoItem {
  border: 1px solid rgb(180, 180, 180);
  box-shadow: 1px 2px 2px rgb(180, 180, 180);
}

.md-input {
  height: auto;
}

.md-field {
  margin: 4px;
  font: 24px Roboto;
}

.completed {
  /* opacity: 70%;   Property removed, as incompatible with heroku and ghPages hosting*/
  text-decoration: line-through;
}
</style>
