<script>
// import HelloWorld from './components/HelloWorld.vue'
// import TheWelcome from './components/TheWelcome.vue'
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks
  },
  data() {
    return {
      tasks: []
    }
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: 'Doctors Appointment',
        day: 'May 5th at 2:30pm',
        reminder: true
      },
      {
        id: 2,
        text: 'Meeting at School',
        day: 'May 6th at 1:30pm',
        reminder: true
      },
      {
        id: 3,
        text: 'Food Shopping',
        day: 'May 7th at 12:30pm',
        reminder: false
      }
    ]
  }
}

</script>

<template>
    <div class="container">
      <Header title="Hello Kennedy"/>
      <Tasks v-bind:tasks="tasks"/>
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
