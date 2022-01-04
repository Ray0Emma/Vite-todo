<template>
  <!-- <h1>{{ msg }}</h1> -->
  <div class="container-md">
    <div class="p-6 max-w-md mx-auto bg-gray-200 rounded-xl shadow-lg">
      <h1 class="text-lg font-semibold mb-3">
        {{
          new Date().toLocaleString("en-us", {
            weekday: "long",
            day: "numeric",
            month: "short",
          })
        }}
      </h1>
      <span class="text-cyan-600 font-base">{{ todos.length }} tasks</span>
      <form @submit.prevent="addTodo()" class="mt-7 mb-7 flex justify-between">
        <input
          class="border-2 border-cyan-600 outline-0 bg-slate-200"
          v-model="newTodo"
          name="newTodo"
          autocomplete="off"
        />
        <button class="ml-3">Add</button>
      </form>
      <ul v-if="todos.length > 0">
        <li
          v-for="(todo, index) in todos"
          :key="index"
          class="flex justify-between"
        >
          <div class="inline">
            <input
              class="mr-2"
              type="checkbox"
              :name="todo.content"
              :checked="todo.done"
              @click="doneTodo(todo)"
            />
            <label :for="todo.content" :class="{ done: todo.done }">{{
              todo.content
            }}</label>
          </div>
          <button @click="removeTodo(index)" class="text-end">Remove</button>
        </li>
      </ul>
      <h4 v-else class="text-center">Empty list.</h4>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  // name: 'Todo',
  setup() {
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Write a blog post",
      },
    ];
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = "";
      }
      saveData();
    }
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
