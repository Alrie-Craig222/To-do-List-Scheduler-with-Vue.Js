<template>
  <div id="filterContainer">
    <select @change="filterTask" name="Filter Task" id="filter">
      <option value="all tasks">View All To-do Tasks</option>
      <option value="completed tasks">Done Tasks</option>
      <option value="incomplete tasks">To-do Tasks</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'Filter',
  methods: {
    filterTask(e) {
      this.$emit('filterTask', e.target.value);
    },
  },
};
</script>

<style scoped>
#filterContainer {
  margin: 2rem 0;
}

select {
  width: 100%;
  text-align: center;
  padding: 0.5rem 0;
  background-color: #dcf7f0;
  color: #53686a;
  font-size: 18px;
  font-weight: 700;
  border: 2px solid #adf9d5;
  outline: none;
}
</style>
