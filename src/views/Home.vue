<template>
  <div id="app"></div>
  <AddTodo v-on:add-todo="addTodo" />
  <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
</template>

<script>
import axios from "axios";
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";
export default {
  name: "Home",
  components: {
    Todos,
    AddTodo,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        // eslint-disable-next-line no-unused-vars
        .then((r) => (this.todos = this.todos.filter((todo) => todo.id !== id)))
        .catch((e) => console.log(e));
    },
    addTodo(todo) {
      const { title, completed } = todo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed,
        })
        .then((resp) => (this.todos = [...this.todos, resp.data]))
        .catch((e) => console.log(e));
    },
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((resp) => (this.todos = resp.data))
      .catch((e) => console.log(e));
  },
};
</script>

<style scoped>
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
