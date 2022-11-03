<template>
  <div class="container">
    <HeaderVue 
      title="Task Tracker" 
      @toggle-add-task="toggleAddTask" 
      :showAddTask="showAddTask" 
    />
    <div v-show="showAddTask">
      <AddTaskVue @add-task="addTask" />
    </div>
    <TasksVue 
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask" 
      :tasks="tasks" 
    />
  </div>
</template>

<script>
import HeaderVue from './components/Header.vue'
import TasksVue from './components/Tasks.vue'
import AddTaskVue from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    HeaderVue,
    TasksVue,
    AddTaskVue
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
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
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
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

<style>

@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}

</style>
