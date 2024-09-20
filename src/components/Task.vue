<template>
  <div v-for="(task, index) in tasks" :key="index" class="taskContainer" :id="task.id">
    <div class="task">
      <p>{{ task.taskName }}</p>
      <p class="dueDate">Due Date: {{ task.date }}</p>
      <span>
        <i :class="task.completed ? 'fas fa-check-double' : 'far fa-square'"
           @click="changeState(task.id)" :id="task.id"></i>
      </span>
    </div>
    <div class="editDelete">
      <span>
        <i @click="deleteTask(task.id)" class="fas fa-trash" id="delete"></i>
      </span>
      <span>
        <i @click="editTask(task.taskName, task.date, task.id)" class="fas fa-edit" id="edit"></i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Task',
  props: {
    tasks: Array,
    deleteTask: Function,
    updateTaskStatus: Function,
  },
  methods: {
    changeState(id) {
      this.tasks.forEach((task) => {
        if (task.id === id) {
          task.completed = !task.completed;
          this.$props.updateTaskStatus(id); // Corrected function call
        }
      });
    },
    deleteTask(id) {
      this.$props.deleteTask(id); // Corrected function call
    },
    editTask(taskName, date, id) {
      this.$bus.$emit('edit-task', { taskName, date, id });
    },
  },
};
</script>

<style scoped>
.taskContainer {
  padding: 1rem;
  background-color: #0cbea1;
  border-radius: 24px;
  margin: 1rem 0;
}

.task {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}

.task p {
  color: #162f40;
}

.dueDate {
  font-size: 0.9rem;
  color: #09dce7;
}

.editDelete {
  display: flex;
  gap: 1rem;
}

.editDelete span {
  cursor: pointer;
  color: #232dff;
}
</style>
