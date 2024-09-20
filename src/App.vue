<template>
  <div id="container">
    <h1 id="logo"><i class="fas fa-clipboard-list"></i> To-Do List Scheduler</h1>
    <Form @addTask="addTask" @updateTask="updateTask"></Form>
    <Filter @filterTask="filterTask" />
    <Tasks :tasks="filteredTasks" :deleteTask="deleteTask" :updateTaskStatus="updateTaskStatus"></Tasks> <!-- Fixed prop bindings -->
  </div>
</template>

<script>
import Form from './components/Form.vue';
import Filter from './components/Filter.vue';
import Tasks from './components/Tasks.vue';

export default {
  name: 'App',
  components: {
    Form,
    Filter,
    Tasks,
  },
  data() {
    return {
      tasks: [],
      filteredTasks: [],
    };
  },
  created() {
    this.tasks = this.getFromStorage();
    this.filteredTasks = this.tasks; // Initialize with all tasks
  },
  methods: {
    addToStorage(task) {
      let tasks = this.getFromStorage();
      tasks.push(task);
      localStorage.setItem('tasks', JSON.stringify(tasks));
    },
    getFromStorage() {
      return JSON.parse(localStorage.getItem('tasks')) || [];
    },
    addTask({ taskName, date }) {
      if (taskName && date) {
        const newTask = {
          taskName,
          date,
          completed: false,
          id: Math.random().toString(16).slice(2),
        };
        this.addToStorage(newTask);
        this.tasks = this.getFromStorage();
        this.filteredTasks = this.tasks; // Refresh task list
      } else {
        alert('Please fill in both task name and date');
      }
    },
    updateTask(updatedTask) {
      let tasks = this.getFromStorage();
      tasks = tasks.map((task) => {
        if (task.id === updatedTask.id) {
          return {
            ...task,
            taskName: updatedTask.taskName,
            date: updatedTask.date,
          };
        }
        return task;
      });
      localStorage.setItem('tasks', JSON.stringify(tasks));
      this.tasks = this.getFromStorage();
      this.filteredTasks = this.tasks; // Refresh task list
    },
    deleteTask(id) {
      const tasks = this.getFromStorage().filter((task) => task.id !== id);
      localStorage.setItem('tasks', JSON.stringify(tasks));
      this.tasks = tasks;
      this.filteredTasks = this.tasks; // Refresh task list
    },
    updateTaskStatus(id) {
      const tasks = this.getFromStorage();
      const updatedTasks = tasks.map((task) => {
        if (task.id === id) {
          task.completed = !task.completed;
        }
        return task;
      });
      localStorage.setItem('tasks', JSON.stringify(updatedTasks));
      this.tasks = updatedTasks;
      this.filteredTasks = this.tasks; // Refresh task list
    },
    filterTask(filter) {
      if (filter === 'completed tasks') {
        this.filteredTasks = this.tasks.filter((task) => task.completed);
      } else if (filter === 'incomplete tasks') {
        this.filteredTasks = this.tasks.filter((task) => !task.completed);
      } else {
        this.filteredTasks = this.tasks;
      }
    },
  },
};
</script>

<style scoped>
#container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

#logo {
  text-align: center;
  color: #067780;
  font-size: 2.5rem;
  font-weight: bold;
  margin: 1rem 0;
}

h1 i {
  color: #000000;
}
</style>
