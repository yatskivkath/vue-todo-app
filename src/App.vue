<script setup>
import { ref, onMounted, watch, computed } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

const todos_asc = computed(() =>
  // eslint-disable-next-line vue/no-side-effects-in-computed-properties
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

const addTodo = () => {
  if (input_content.value.trim() == "" || input_category.value == null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date(),
  });
};

watch(
  todos,
  (newValue) => {
    localStorage.setItem("todos", newValue);
  },
  { deep: true }
);

watch(name, (newValue) => {
  localStorage.setItem("name", newValue);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
});
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
            value="business"
            v-model="input_category"
          />
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
      </div>

      <input type="submit" value="Add Todo" />
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
