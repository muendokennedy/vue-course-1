<script>
// import HelloWorld from './components/HelloWorld.vue'
// import TheWelcome from './components/TheWelcome.vue'
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTask from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
      ShowAddTask: false
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
  methods: {
    toggleAddTask(){
      this.ShowAddTask = !this.ShowAddTask
    },
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
    <div class="container">
      <Header title="Task App" v-on:toggle-add-task="toggleAddTask" v-bind:ShowAddTask="ShowAddTask"/>
      <div v-if="ShowAddTask">
        <AddTask v-on:add-task="addTask"/>
      </div>
      <Tasks v-on:delete-task="deleteTask" v-on:toggle-reminder="toggleReminder" v-bind:tasks="tasks"/>
    </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}
.container{
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn{
  display: inline-block;
  border: none;
  padding: 10px 20px;
  background: black;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  margin: 5px;
  font-size: 15px;
  cursor: pointer;
  font-family: inherit;
}
.btn:focus{
  outline: none;
}
.btn:active{
  transform: scale(0.98);
}
.btn-block{
  display: block;
  width: 100%;
}
</style>
