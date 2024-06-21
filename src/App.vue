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
        <div class="d-flex">
          <form @submit.prevent="transformToText(index)">
            <span @click="makeEditable(index)" v-if="!editStatus[index]">{{ task }}</span>
            <input v-if="editStatus[index]" type="text" v-model="tasks[index]">
          </form>
          <button class="btn btn-danger" @click="removeTask(index)">Delete</button>
        </div>
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
      tasks: [],
      editStatus: []
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push(this.newTask);
        this.editStatus.push(false)
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.$delete(this.tasks, index);
      this.editStatus.splice(index, 1);
    },
    makeEditable(index) {
      this.$set(this.editStatus, index, true);
    },
    transformToText(index) {
      this.$set(this.editStatus, index, false)
    },
  },
}
</script>

<style>

</style>
