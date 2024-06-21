<template>
  <div id="app">
    <h1>To-Do List</h1>
    <form @submit.prevent="addTask">
      <input
          type="text"
          v-model="newTask"
          placeholder="Add new task"
      />
      <button class="btn btn-info">Add task</button>
    </form>

    <ol>
      <li v-for="(task, index) in tasks" :key="index">
        {{task}}
        <button class="btn btn-danger" @click="removeTask(index)">Delete</button>
      </li>
    </ol>
  </div>
</template>

<script>

import 'bootstrap/dist/css/bootstrap.min.css';

export default {
  name: 'App',
  data() {
    return {
      newTask: '',
      tasks: []
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push(this.newTask);
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.$delete(this.tasks, index);
      // this.tasks.splice(index, 1);
    },
  },
}
</script>

<style>

</style>
