<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Header />
    <AddTodo 
      v-on:add-todo="addTodo"
    />
    <Todos 
      v-bind:todos="todos" 
      v-on:del-todo="deleteTodo"
    />
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Todos from './components/Todos';
import Header from './components/layout/Header';
import AddTodo from './components/AddTodo';

import axios from 'axios';

export default {
  name: 'App',
  components: {
    HelloWorld,
    Header,
    Todos,
    AddTodo
  },
  methods: {
    deleteTodo(id) {
      axios.delete(
        `https://jsonplaceholder.typicode.com/todos/${id}`
      ).then(
        this.todos = this.todos.filter(todo => todo.id !== id)
      ).catch(
        err => console.log(err)
      )
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      axios.post(
        'https://jsonplaceholder.typicode.com/todos',
        {
          title,
          completed
        }
      )
      .then(
        res => this.todos = [...this.todos, res.data]
      ).catch(
        err => console.log(err)
      )
    }
  },
  created() {
    axios.get(
      'https://jsonplaceholder.typicode.com/todos?_limit=10'
    )
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
  },
  data() {
    return {
      todos: []
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
