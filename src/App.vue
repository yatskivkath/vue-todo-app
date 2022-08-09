<script>
export default {
  data() {
    return {
      name: "",
      input_content: "",
      input_category: null,
      todos: [],
    };
  },
  computed: {
    todos_asc() {
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      return this.todos.sort((a, b) => {
        return a.createdAt - b.createdAt;
      });
    },
  },
  methods: {
    addTodo() {
      this.todos.push({
        content: this.input_content,
        category: this.input_category,
        done: false,
        createdAt: new Date(),
      });

      this.input_content = "";
      this.input_category = null;
    },
    removeTodo(todo) {
      this.todos = this.todos.filter(t => t !== todo);
    },
    isDone(item) {
      return item.done ? "done" : "";
    },
  },
  watch: {
    name(newName) {
      localStorage.setItem("name", newName);
    },
    todos: {
      handler(newTodos) {
        localStorage.setItem("todos", JSON.stringify(newTodos));
      },
      deep: true,
    },
  },
  mounted() {
    if (localStorage.name) {
      this.name = localStorage.getItem("name");
    }
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
};
</script>

<template>
  <section class="greeting">
    <h2 class="greeting__title">
      Hello, <input type="text" placeholder="Type Your Name" v-model="name" />
    </h2>
  </section>

  <section class="create-todo">
    <h3 class="create-todo__title">Create a Todo</h3>

    <form @submit.prevent="addTodo">
      <h4>What's on your todo list?</h4>
      <input
        type="text"
        placeholder="e.g. make a video"
        v-model="input_content"
      />

      <h4>Pick a Category</h4>
      <div class="create-todo__options">
        <label>
          <input
            type="radio"
            name="category"
            value="business"
            v-model="input_category"
          />
          <span class="bubble business"></span>
          <div>Business</div>
        </label>

        <label>
          <input
            type="radio"
            name="category"
            value="personal"
            v-model="input_category"
          />
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
      </div>

      <input type="submit" value="Add Todo" />

      <section class="todo-list">
        <h3 class="todo-list__title">ToDo List</h3>
        <div class="todo-list__list">
          <div
            v-for="todo in todos_asc"
            :key="todo.createdAt"
            :class="`todo-list__item ${isDone(todo)}`"
          >
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`" />
            </label>

            <div class="todo-list__item-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </form>
  </section>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
