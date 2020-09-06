<template>
  <div id="app">
    <AddTodo @add-todo="addTodo"/>
    <Todos :todos="todos" @del-todo="deleteTodo"/>
  </div>
</template>

<script>

import Todos from '../components/Todos'
import AddTodo from '../components/AddTodo'
import axios from 'axios'
// import VueAxios from 'vue-axios'

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: [] 
    }
    },
    methods: {
      // id sent as payload in emit
      deleteTodo(id) {
        axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
             // filter loops like foreach but based on condition
        // we want all except one with that id
        .then(this.todos = this.todos.filter(todo => todo.id !== id))
        .catch(err => console.log(err));
      },
      addTodo(newTodo) {

        const { title, completed } = newTodo;
        axios.post('https://jsonplaceholder.typicode.com/todos', {
          title, 
          completed
        })
        // when you make post req it gives you back the new todo 
        // along with the id it gave to it
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err))
      },
    
    },
    created() {
      //axios used for HTTP requests
      axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
      // res is returned from API
      .then(res => {console.log(res);this.todos = res.data})
      .catch(err => console.log(err))
    }
    
  }
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
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
