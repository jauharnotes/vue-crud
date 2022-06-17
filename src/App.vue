<script>
import ListTodos from './components/ListTodos.vue';
export default {
  components: { ListTodos },
  data() {
    return {
      todo: '',
      todos: [],
    };
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todos'));
  },
  computed: {
    totalTodo() {
      return this.todos.length;
    },
  },
  methods: {
    add() {
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.todo = '';
      this.saveToLocalStorage();
    },
    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      this.saveToLocalStorage();
    },
    doneTodo(doneIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == doneIndex) {
          item.isDone = !item.isDone;
        }

        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
  },
};
</script>

<template>
  <div class="container" style="margin-top: 20px">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input
              v-model="todo"
              @keyup.enter="add"
              type="text"
              class="form-control"
            />
          </div>
          <div class="col-2">
            <button @click="add" class="btn btn-success">ADD</button>
          </div>
        </div>
      </div>
    </div>
    <list-todos :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
    <br />
    <small>Total Todo: {{ totalTodo }}</small>
  </div>
</template>

<style></style>
