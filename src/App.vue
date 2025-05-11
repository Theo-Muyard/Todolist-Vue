<template>
  <main class="container">
    <form @submit.prevent="addTask">
      <fieldset role="group">
        <input v-model="taskName" type="text" placeholder="Enter a task..." />
        <button :disabled="taskName.trim().length < 1">Add</button>
      </fieldset>
    </form>

    <div v-if="filteredTasks.length === 0">You don't have any tasks</div>

    <div v-else>
      <ul>
        <li
          v-for="task in filteredTasks"
          :key="task.date"
          :class="{ completed: task.completed }"
        >
          <input v-model="task.completed" type="checkbox" />
          {{ task.title }}
        </li>
      </ul>
    </div>
    <label>
      <input v-model="hideTasksCompleted" type="checkbox" />
      Hide completed tasks
    </label>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";

const taskName = ref("");
const hideTasksCompleted = ref(false);

const tasks = ref([
  {
    title: "Cook dinner",
    completed: false,
    date: 1,
  },
]);

const addTask = () => {
  if (taskName.value.trim().length < 1) return;

  tasks.value.push({
    title: taskName.value.trim(),
    completed: false,
    date: Date.now(),
  });

  taskName.value = "";
};

const filteredTasks = computed(() => {
  const sorted = [...tasks.value].sort((a, b) =>
    a.completed === b.completed ? 0 : a.completed ? 1 : -1
  );

  return hideTasksCompleted.value
    ? sorted.filter((task) => !task.completed)
    : sorted;
});
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  opacity: 0.5;
}
</style>
