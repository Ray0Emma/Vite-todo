<template>
  <div class="container-md">
    <div class="p-8 max-w-md mx-auto bg-light rounded-xl shadow-lg">
      <h1 class="text-lg font-semibold mb-2">
        {{ date }}
      </h1>
      <span class="text-bleuby font-medium">{{ todos.length }} tasks</span>
      <form @submit.prevent="addTodo()" class="mt-7 mb-7 flex justify-between">
        <input
          class="border focus:border-bleuby border-midnight outline-0 w-96"
          v-model="newTodo"
          name="newTodo"
          autocomplete="off"
        />
        <button class="ml-3">
          <img src="https://img.icons8.com/ios-glyphs/52/5C76E4/plus.png" />
        </button>
      </form>
      <ul v-if="todos.length > 0">
        <li
          v-for="(todo, index) in todos"
          :key="index"
          class="flex justify-between mb-3"
        >
          <div class="inline">
            <input
              class="mr-2 w-5 h-5 inline-block align-text-bottom checked:text-red-100"
              type="radio"
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
  name: "ToDo",
  setup() {
    const date = new Date().toLocaleString("en-us", {
      weekday: "long",
      day: "numeric",
      month: "short",
    });
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Morning walk",
      },
    ];
    const todosData = JSON.parse(localStorage.getItem("tasks")) || defaultData;
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
      localStorage.setItem("tasks", storageData);
    }
    return {
      date,
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
