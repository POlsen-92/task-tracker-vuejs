<template>
  <div :key="task.id" v-for="task in tasks">
    <TaskVue 
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"  
      :task="task" 
    />
  </div>
</template>

<script>
  import TaskVue from './Task.vue';

  export default {
    name: 'TasksVue',
    props: {
      tasks: Array
    },
    components: {
      TaskVue
    },
    emits: ['delete-task', 'toggle-reminder']
  }
</script>

<style scoped>
  
</style>