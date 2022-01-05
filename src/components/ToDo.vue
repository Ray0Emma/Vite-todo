<template>
  <div class="container-md">
    <div
      class="p-7 sm:p-8 max-w-mobile md:max-w-lg mx-auto bg-dark rounded-xl shadow-lg"
    >
      <h1 class="text-lg font-semibold mb-2">
        {{ date }}
      </h1>
      <span class="text-bleuby font-medium">{{ todos.length }} tasks</span>
      <form @submit.prevent="addTodo()" class="mt-7 mb-7">
        <input
          class="border bg-input focus:border-bleuby border-border outline-0 w-full py-2 px-5 rounded"
          v-model="newTodo"
          name="newTodo"
          placeholder="Add a task..."
          autocomplete="off"
        />
      </form>
      <ul v-if="todos.length > 0" class="mb-3">
        <li
          v-for="(todo, index) in todos"
          :key="index"
          class="flex justify-between mb-3"
        >
          <div class="inline">
            <input
              class="mr-2 w-5 h-5 inline-block align-text-bottom"
              type="radio"
              :name="todo.content"
              :checked="todo.done"
              @click="doneTodo(todo)"
            />
            <label :for="todo.content" :class="{ done: todo.done }">{{
              todo.content
            }}</label>
          </div>
          <button @click="removeTodo(index)" class="text-end">
            <img src="https://img.icons8.com/ios-filled/20/5C76E4/trash.png" />
          </button>
        </li>
      </ul>
      <h4 v-else class="text-center mb-5">No Tasks To Do.</h4>
      <!-- <div v-if="completed.length > 0" class="mb-3">
        <hr class="text-border" />
        <li
          v-for="(comp, index) in completed"
          :key="index"
          class="flex justify-between mb-3"
        >
          <label :for="comp.content" class="done">{{ comp.content }}</label>
          <button @click="removeTodo(index)" class="text-end">
            <img src="https://img.icons8.com/ios-filled/20/5C76E4/trash.png" />
          </button>
        </li>
      </div> -->
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
    // const completed = ref([]);
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
      // completed.value.push({
      //   done: true,
      //   content: todo.content,
      // });
      saveData();
    }
    function removeTodo(index) {
      todos.value.splice(index, 1);
      // completed.value.splice(index, 1);
      saveData();
    }
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("tasks", storageData);
    }
    return {
      date,
      // completed,
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
input[type="radio"] {
  filter: invert(80%) saturate(0%);
}
</style>
