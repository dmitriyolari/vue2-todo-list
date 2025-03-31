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

    <div>
      <button class="btn btn-secondary mr-2" @click="filter='all'">All</button>
      <button class="btn btn-primary mr-2" @click="filter='active'">Active</button>
      <button class="btn btn-success" @click="filter='completed'">Completed</button>
    </div>
    <ol>
      <li v-for="(task, index) in filteredTasks" :key="index">
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
    <button v-if="tasks.length !== 0" class="btn btn-danger" @click="removeAllTasks()">Delete All</button>

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
      editStatus: [],
      filter: 'all'
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
    removeAllTasks() {
      this.tasks = [];
      this.editStatus = [];
    },
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'active') {
        return this.tasks.filter(task => !task.done)
      } else if (this.filter === 'completed') {
        return this.tasks.filter(task => task.done)
      }
      return this.tasks;
    }
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
