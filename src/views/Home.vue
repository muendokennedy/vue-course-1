<script>
import AddTask from '../components/AddTask.vue'
import Tasks from '../components/Tasks.vue'

export default {
  name: 'Home',
  props: {
    ShowAddTask: Boolean
  },
  components: {
    AddTask,
    Tasks
  },
  data() {
    return {
      tasks: [],
    }
},

async created() {
    this.tasks = await this.fetchTasks()
  },
  methods: {
    async addTask(task){

      const response = await fetch('http://localhost:5000/tasks',{
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })

      const data = await response.json()

      this.tasks = [...this.tasks, data]

    },
    async deleteTask(id){

      const response = await fetch(`http://localhost:5000/tasks/${id}`,{
        method: 'DELETE',
      })

      response.status === 200 ? this.tasks = this.tasks.filter((task) => (
        task.id !== id
      )) : alert('Error Deleting This Task')
    },
    async toggleReminder(id){

      const taskToToggle = await this.fetchTask(id)

      const updateTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const response = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updateTask)
      })

      const data = await response.json()

      this.tasks = this.tasks.map((task) => 
        task.id === id ? {...task, reminder: data.reminder } : task
      )
    },

    async fetchTasks(){

      const response = await fetch('http://localhost:5000/tasks')
      
      const data = await response.json()

      return data
  },
    async fetchTask(id){

      const response = await fetch(`http://localhost:5000/tasks/${id}`)
      
      const data = await response.json()

      return data
  }
}
}

</script>

<template>
  <AddTask v-show="ShowAddTask" v-on:add-task="addTask"/>
  <Tasks v-on:delete-task="deleteTask" v-on:toggle-reminder="toggleReminder" v-bind:tasks="tasks"/>
</template>