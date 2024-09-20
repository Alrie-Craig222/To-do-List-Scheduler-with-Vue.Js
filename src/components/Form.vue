<template>
  <div id="formGroup">
    <form v-on:submit.prevent="submitTask" id="formSubmit">
      <label for="taskInput">Task Title</label>
      <input v-model="taskInput" type="text" id="taskInput" placeholder="Enter Task Title" />

      <label for="dateInput">Date</label>
      <input v-model="dateInput" type="date" id="dateInput" placeholder="Select Date" />

      <button type="submit">{{ buttonText }}</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Form',
  props: ['updateTask'],
  data() {
    return {
      taskInput: '',
      dateInput: '',
      buttonText: 'Add Task',
      taskId: null,
    };
  },
  created() {
    this.$bus.$on('edit-task', ({ taskName, date, id }) => {
      this.taskInput = taskName;
      this.dateInput = date;
      this.taskId = id;
      this.buttonText = 'Save Changes';
    });
  },
  methods: {
    submitTask() {
      if (this.taskId !== null) {
        this.$emit('updateTask', {
          id: this.taskId,
          taskName: this.taskInput,
          date: this.dateInput,
        });
        this.buttonText = 'Add Task';
        this.taskId = null;
      } else {
        this.$emit('addTask', {
          taskName: this.taskInput,
          date: this.dateInput,
        });
      }
      this.taskInput = '';
      this.dateInput = '';
    },
  },
};
</script>

<style scoped>
#formGroup {
  margin: 2rem 0;
  padding: 1rem;
  background-color: #fff;
  border-radius: 8px;
}

#formSubmit {
  display: flex;
  flex-direction: column;
}

#formSubmit input {
  margin: 0.3rem 0 1rem 0;
  padding: 0.5rem;
  outline: none;
  background-color: #dcf7f0;
  color: #53686a;
  border: 2px solid #adf9d5;
  border-radius: 8px;
}

#formSubmit button {
  padding: 0.5rem 0;
  background-color: #232dff;
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 17px;
  cursor: pointer;
}

#formSubmit button:hover {
  background-color: #0769fe;
}
</style>
