<template>
  <header>
    <h1>{{ title }}</h1>
    <ButtonVue 
      :text="showAddTask ? 'Close':'Add Task'" 
      :color="showAddTask ? 'red':'green'"
      @btn-click="$emit('toggle-add-task')" 
    /> 
  </header>
</template>

<script>
  import ButtonVue from './Button.vue';

  export default {
    name: 'HeaderVue',
    props: {
      title: {
        type: String,
        default: 'Hello World'
      },
      showAddTask: Boolean
    },
    components: {
      ButtonVue,
    },
    emits: ['toggle-add-task']
  }
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>