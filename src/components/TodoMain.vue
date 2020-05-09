<template>
  <div>
    <div>
      <h4>Todo List</h4>
    </div>
    <table class="todo-table">
      <tr>
        <td><button @click="deleteTodo">삭제</button></td>
        <td colspan="3">
          <input
            type="text"
            class="input-todo"
            placeholder="내용을 입력해주세요 ..."
            ref="newTodoTxt"
            v-model="newTodoTxt"
          />
        </td>
        <td>
          <button @click="addTodo">등록</button>
        </td>
      </tr>
      <tr class="todo-header">
        <td>
          <input type="checkbox" v-bind:checked="chkAll" @click="checkAll" />
        </td>
        <td class="todo-header-txt">No</td>
        <td>Title</td>
        <td>UserId</td>
        <td>Status</td>
      </tr>
      <tr
        v-for="todo in todos"
        :key="todo.id"
        class="todo-list"
        @click="isCompleted(todo.id, todo.completed)"
      >
        <td><input type="checkbox" v-bind:checked="todo.completed" /></td>
        <td class="todo-id">{{ todo.id }}</td>
        <td class="todo-title" @click="toggleCompleted(todo.id)" ref="todo">
          {{ todo.title }}
        </td>
        <td class="todo-userid">{{ todo.userId }}</td>
        <td class="todo-completed">{{ todo.completed }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TodoMain",
  data() {
    return {
      todos: [],
      chkAll: false,
      newTodoTxt: null,
    };
  },
  mounted() {
    this._initTodos();
  },
  methods: {
    _initTodos() {
      axios.get("https://jsonplaceholder.typicode.com/todos").then((result) => {
        // this.todos = result.data;

        this.todos = result.data.splice(0, 10);

        console.log(this.todos);
      });
    },

    //event
    isCompleted(id, isCompleted) {
      if (isCompleted != null) {
        this.todos.filter((todo) => {
          if (todo.id === id) {
            if (todo.completed != true) {
              todo.completed = true;
            } else {
              todo.completed = false;
            }
          }
        });
      }
    },

    checkAll() {
      if (this.chkAll != true) {
        this.chkAll = true;
        this.todos.forEach((todo) => (todo.completed = true));
      } else {
        this.chkAll = false;
        this.todos.forEach((todo) => (todo.completed = false));
      }
      console.log(this.chkAll);
    },

    toggleCompleted(id) {
      console.log(id);
      let el = this.$refs.todo;
      console.log(el);
    },

    _initNewId() {
      //.max() 함수를 이용하여 새로운 id를 입력하는 방법

      let maxId = 0;
      for (let i = 0; i < this.todos.length; i++) {
        maxId = Math.max(this.todos[i].id);
      }

      let newId = maxId + 1;
      return newId;
    },

    addTodo() {
      this.todos.push({
        completed: false,
        id: this._initNewId(),
        title: this.newTodoTxt,
        userId: 1,
      });
    },

    deleteTodo() {
      console.log("delete todo");

      let deleteIds = [];

      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].completed === true) {
          deleteIds.push(this.todos[i].id);
        }
      }
      console.log(deleteIds);
    },
  },
  computed: {},
};
</script>

<style>
.input-todo {
  width: 400px;
}

.todo-table {
  width: 650px;
  border-collapse: collapse;
  margin: 0 auto;
}

.todo-header {
  height: 30px;
  background-color: lightyellow;
}

.todo-list {
  height: 30px;
}

.todo-list:hover {
  background-color: #f4f4f4;
  cursor: pointer;
}

.todo-title {
  text-align: left;
  /* text-decoration: line-through; */
}

.todo-completed,
.todo-userid,
.todo-id {
  width: 50px;
}
</style>
