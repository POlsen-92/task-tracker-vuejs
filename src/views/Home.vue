<template>
  <AddTaskVue
    v-show="showAddTask" 
    @add-task="addTask" 
  />
  <TasksVue 
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask" 
    :tasks="tasks" 
  />
</template>

<script>
  import AddTaskVue from '@/components/AddTask.vue';
  import TasksVue from '@/components/Tasks.vue';

  export default {
    name: 'HomeVue',
    components: {
      AddTaskVue,
      TasksVue
    },
    props: {
      showAddTask: Boolean
    },
    data() {
      return {
        tasks: [],
      }
    },
    methods: {
    async addTask(newTask){
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: { 
          'Content-type': 'application/json',
        },
        body: JSON.stringify(newTask)
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id){
      if(confirm('Are you sure?')){
        const res = await fetch(`api/tasks/${id}`, {method: 'DELETE'})

        if(res.status == 200){
          this.tasks = this.tasks.filter((task)=>{
            return task.id !== id
          })
        } else {
          alert('Error: Task Not Deleted')
        }
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: { 
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask)
      })

      if(res.status == 200){
        this.tasks = this.tasks.map((task)=>{
          if(task.id === id){
            task.reminder = !task.reminder
          }
          return task
        })
      } else {
        alert('Error: Task Not Updated')
      }
    },
    async fetchTasks () {
      const res = await fetch('api/tasks')

      const data = await res.json()

      return data
    },
    async fetchTask (id) {
      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
  }

</script>