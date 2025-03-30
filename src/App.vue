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
        <div class="d-flex align-items-center">
          <input type="checkbox" v-model="task.done">
          <form @submit.prevent="transformToText(index)">
            <span
                @click="makeEditable(index)"
                v-if="!editStatus[index]"
                :class="{'text-muted': task.done, 'text-decoration-line-through': task.done}"
            >{{ task.text }}</span>
            <input v-if="editStatus[index]" type="text" v-model="tasks[index].text">
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
        this.tasks.push({
          text: this.newTask,
          done: false
        });
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
  watch: {
    tasks: {
      handler(newTasks) {
        localStorage.setItem('tasks', JSON.stringify(newTasks))
        localStorage.setItem('editStatus', JSON.stringify(newTasks))
      },
      deep: true
    }
  },
  mounted() {
    const savedTasks = localStorage.getItem('tasks');
    const savedStatus = localStorage.getItem('editStatus');

    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks)
    }

    if (savedStatus) {
      this.editStatus = JSON.parse(savedStatus)
    }
  }
}
</script>

<style>

</style>
