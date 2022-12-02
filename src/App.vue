<template>
  <div id="app">
    <article class="content">
      <h1 class="content__title">To Do list manager</h1>
      <AddToDoButton @add-todo-event="addToDoItem" />
    </article>
    <br/>
    <article class="content__list" v-if="todoEntries?.length">
      <h1 class="content__title">To Do List</h1>
      <ToDoList
        :todoEntries="todoEntries"
        @delete-todo-event="deleteToDoItem"
        @edit-todo-event="updateToDoItem" />
    </article>
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue';
import AddToDoButton from './components/AddToDoButton.vue';
export default {
  name: 'App',
  components: {
    AddToDoButton,
    ToDoList
  },
  data() {
    return {
      todoEntries: [],
    }
  },
  methods: {
    addToDoItem(newToDoItem) {
      let id;
      if (this.todoEntries?.length)
        id = Math.max.apply(Math, this.todoEntries.map((obj) => {return obj.id;})) + 1;
      else
        id = 1;
      newToDoItem.id = id;
      this.todoEntries = [...this.todoEntries, newToDoItem];
    },
    deleteToDoItem(toDoId){
      this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)
    },
    updateToDoItem(data) {
      this.todoEntries.forEach(todo => {
        if (todo.id === data.id)
          todo.title = data.value;
      });
    }
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
  }

  html,
  body {
    height: 100%;
  }

  body {
    font-family: sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #0c3b73;
    font-size: 1.3rem;
  }

  .content {
    background-color: #4a58b2;
    width: 768px;
    max-width: 768px;
    padding: 3rem;
    box-shadow: 2px 2px 3px gray;
    border-radius: 1rem;
  }

  .content__list {
    background-color: #a879fa;
    width: 768px;
    max-width: 768px;
    padding: 3rem;
    box-shadow: 2px 2px 3px gray;
    border-radius: 1rem;
  }

  .content__title {
    margin: 1rem;
    text-align: center;
  }
</style>
