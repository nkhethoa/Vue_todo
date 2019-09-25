
<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos ="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      //array of todos from JSON 
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => this.todos = this.todos.filter(todo =>  todo.id !== id))
      .catch(err => console.log(err));
      
    },
    addTodo(newTodo) {
      //destructuring method
      const { title, completed } = newTodo;

      axios.post ('https://jsonplaceholder.typicode.com/todos', {
        title, //title:title
        completed
      })
        .then( res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err))
    }
  },
  //populate the array with fake data
  created() {
    axios.get ('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(res => this.todos = res.data)
    .catch(err => console.log(err));
  }
}
</script>
