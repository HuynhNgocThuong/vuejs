<template>
  <!-- Template Conditionals -->
  <!-- App.vue -> props value to Home -> attributes showAddTask -->
  <AddTask @add-task="addTask" v-show="showAddTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';
export default {
  name: 'Home',
  components: {
    Tasks,
    AddTask,
  },
  props: {
    showAddTask: Boolean,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    // Refactoring to Use The Backend
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('Error Deleting task.');
      }
    },
    async toggleReminder(id) {
      const taskToggle = await this.fetchTask(id);
      const updateTask = { ...taskToggle, reminder: !taskToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updateTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !data.reminder } : task
      );
    },
    // JSON-Server Setup
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
    async addTask(task) {
      // Post to create new task on server side
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });
      //Update data tasks
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
  },
  // Task Data & created() Method
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
