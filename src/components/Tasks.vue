<template>
  <div class="tasksContainer">
    <Task 
      v-if="tasks.length > 0"
      :tasks="tasks"
      :deleteTask="deleteTask"
      :updateTaskStatus="updateTaskStatus"
    />
    <div v-else class="noTasks">No Tasks Available</div>
  </div>
</template>

<script>
import Task from './Task.vue';

export default {
  name: 'Tasks',
  components: {
    Task,
  },
  props: {
    tasks: Array,
    deleteTask: Function,
    updateTaskStatus: Function,
  },
};
</script>

<style scoped>
.tasksContainer {
  margin: 2rem 0;
  padding: 1rem;
  background-color: #1c9da7;
  border-radius: 8px;
  max-height: 200px;
  overflow: hidden scroll;
}

.tasksContainer::-webkit-scrollbar {
  width: 5px;
  height: 8px;
  background-color: #a1faeb;
}

.tasksContainer::-webkit-scrollbar-thumb {
  background: #0ac7e9;
  border-radius: 4px;
}

.noTasks {
  text-align: center;
  font-size: 1.5rem;
  font-weight: 500;
}
</style>
