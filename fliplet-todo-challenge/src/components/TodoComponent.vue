<template>
  <div>
    <h2>To do: ({{ incompleteTodos.length }})</h2>
    <input type="text" v-model="todoText" @keyup.enter="addNewTodo" placeholder="Type something and press ENTER" />
    <div class="errorMessageContainer">
      <span class="errorMessage" v-if="error">{{ errorMessage }}</span>
    </div>
    <Container tag="ol" group-name="tasks" :get-child-payload="(i) => incompleteTodos[i]" @drop="handleDrop(false, $event)">
      <Draggable v-for="todo in incompleteTodos" :key="todo.id">
        <li>
          <label>
            <input type="checkbox" v-model="todo.completed" />
            <span>{{ todo.text }}</span>
            <a href="#" class="remove" @click="removeTodo(todo.id)">Remove</a>
          </label>
        </li>
      </Draggable>
    </Container>

    <hr />
    <h2>Completed items: ({{ completedTodos.length }})</h2>

    <Container tag="ol" group-name="tasks" :get-child-payload="(i) => completedTodos[i]" @drop="handleDrop(true, $event)">
      <Draggable v-for="completedTodo in completedTodos" :key="completedTodo.id">
        <li>
          <label>
            <input type="checkbox" v-model="completedTodo.completed" />
            <span>{{ completedTodo.text }}</span>
            <a href="#" class="remove" @click="removeTodo(completedTodo.id)">Remove</a>
          </label>
        </li>
      </Draggable>
    </Container>
  </div>
</template>

<script>
import { Container, Draggable } from "vue-smooth-dnd";

export default {
  name: "TodoComponent",
  components: {
    Container,
    Draggable,
  },
  data() {
    return {
      todoText: "",
      todos: [
        { id: 1745609640733, text: "Send job application", completed: true },
        { id: 1745609652474, text: "Learn about Vue", completed: false },
        { id: 1745609670848, text: "Learn about Fliplet", completed: false },
        {
          id: 1745609690419,
          text: "Play around in JSFiddle",
          completed: false,
        },
        {
          id: 1745609701024,
          text: "Show us what you've got",
          completed: false,
        },
      ],
      error: false,
      errorMessage: "",
    };
  },
  computed: {
    incompleteTodos() {
      return this.todos.filter((todo) => !todo.completed);
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.completed);
    },
  },
  methods: {
    addNewTodo() {
      /* Ensure user actually wrote a todo and it has contents */
      const todoText = this.todoText.trim();

      if (!todoText) {
        this.error = true;
        this.errorMessage = "Woops, looks like you tried to create a todo but you didn't write anything.";
        return;
      }

      /* Create and the task */
      const randomId = Math.floor(Date.now() + Math.random());
      this.todos.push({
        id: randomId, // temporary id until DB would provide real one post-creation in production
        text: todoText,
        completed: false,
      });

      /* Handling successful adding of todo */
      this.todoText = "";
      this.error = false;
      this.errorMessage = "";
    },

    removeTodo(todoId) {
      this.todos = this.todos.filter((todo) => todo.id !== todoId);
    },

    handleDrop(isCompleted, dropResult) {
      const { addedIndex, removedIndex, payload } = dropResult;

      /* Handle moving between lists */
      if (addedIndex !== null && removedIndex === null) {
        payload.completed = isCompleted;

        const originalIndex = this.todos.indexOf(payload);
        this.todos.splice(originalIndex, 1);

        const insertionIndex = this.findOriginalTodoIndex(addedIndex, isCompleted);
        this.todos.splice(insertionIndex, 0, payload);
        return;
      }

      /* Handle re-ordering within the same list */
      if (addedIndex !== null && removedIndex !== null) {
        const fromPosition = this.findOriginalTodoIndex(removedIndex, isCompleted);
        const toPosition = this.findOriginalTodoIndex(addedIndex, isCompleted);

        const [moved] = this.todos.splice(fromPosition, 1);
        this.todos.splice(toPosition, 0, moved);
      }
    },

    /* Uses index from one of the computed lists to find position of the same todo in the original todos array */
    findOriginalTodoIndex(localIndex, isCompleted) {
      let matchCount = 0;
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].completed === isCompleted) {
          if (matchCount === localIndex) return i;
          matchCount++;
        }
      }
      return this.todos.length;
    },
  },
};
</script>

<style scoped>
li {
  margin: 8px 0;
}

h2 {
  font-weight: bold;
  margin-bottom: 15px;
}

del {
  color: rgba(0, 0, 0, 0.3);
}

input[type="text"] {
  padding: 10px;
  width: 200px;
}

label .remove {
  display: none;
  color: red;
}

label:hover .remove {
  display: inline-block;
  margin-left: 10px;
  color: red;
  opacity: 0.5;
}

.errorMessageContainer {
  margin-top: 5px;
}

.errorMessage {
  color: red;
  font-size: 14px;
}
</style>
