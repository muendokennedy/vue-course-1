<template>
  <header>
    <h1>{{ title }}</h1>
    <Button v-show="homePage" v-on:btn-click="this.$emit('toggle-add-task')" v-bind:text="ShowAddTask ? 'Close' : 'Add Task'" v-bind:color="ShowAddTask ? 'red' : 'green'"/>
  </header>
</template>

<script>
import Button from './Button.vue'

export default {
  name: 'Header',
  props: {
    title: String,
    ShowAddTask: Boolean
  },
  components: {
    Button
  },
  computed: {
    homePage(){
      if(this.$route.path === '/'){
        return true
      } else {
        return false
      }
    }
  }
}
</script>

<style scoped>
header{
  display: flex;
  justify-content: space-between;
  align-self: center;
  margin-bottom: 20px;
}
</style>