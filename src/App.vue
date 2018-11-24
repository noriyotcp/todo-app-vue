<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <div class="todolist not-done">
          <h1>Todos</h1>
          <add-todo />
          <hr>
          <todo-list-view :todos="unFinishedTodos" />
          <div class="todo-footer">
            <strong><span class="count-todos"></span></strong> {{ unFinishedTodos.length }} Items Left
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <finished-todos :todos="finishedTodos" />
      </div>
    </div>
  </div>
</template>

<script>
import TodoListView from './components/TodoListView.vue'
import AddTodo from './components/AddTodo.vue'
import FinishedTodos from './components/FinishedTodos.vue'
import { todos } from './seed'
import EventBus from './EventBus'

export default {
  name: "App",
  components: {
    TodoListView,
    AddTodo,
    FinishedTodos
  },
  data () {
    return {
      todos: todos
    }
  },
  created () {
    EventBus.$on('add-todo', event => this.addTodo(event))
    EventBus.$on('complete-todo', event => this.completeTodo(event))
    EventBus.$on('delete-todo', event => this.deleteTodo(event))
  },
  methods: {
    addTodo (event) {
      this.todos.push(event)
    },
    completeTodo (event) {
      const index = this.todos.findIndex(todo => todo.id === event.id)
      this.todos[index].completed = true
    },
    deleteTodo (event) {
      const index = this.todos.findIndex(todo => todo.id === event.id)
      this.todos.splice(index, 1)
    }
  },
  computed: {
    finishedTodos () {
      return this.todos.filter(todo => todo.completed === true)
    },
    unFinishedTodos () {
      return this.todos.filter(todo => todo.completed === false)
    }
  }
};
</script>

<style>
body {
  background-color: #eeeeee;
}
.todolist {
  background-color: #fff;
  padding: 20px 20px 10px 20px;
  margin-top: 30px;
}
.todolist h1 {
  margin: 0;
  padding-bottom: 20px;
  text-align: center;
}
.form-control {
  border-radius: 0;
}
li.ui-state-default {
  background: #fff;
  border: none;
  border-bottom: 1px solid #ddd;
}

li.ui-state-default:last-child {
  border-bottom: none;
}

.todo-footer {
  background-color: #f4fce8;
  margin: 0 -20px -10px -20px;
  padding: 10px 20px;
}
#done-items li {
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
  text-decoration: line-through;
}
#done-items li:last-child {
  border-bottom: none;
}
#checkAll {
  margin-top: 10px;
}
</style>
