<template>
  <!-- For to render item task -->
  <!-- Tasks Component & v-for Loop, need :key=id -->
  <div v-for="task in tasks" :key="task.id">
    <!--  Render Single Task Componen -->
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<script>
import Task from './Task.vue';
export default {
  name: 'Tasks',
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-reminder'],
};
</script>
