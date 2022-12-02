<template>
  <div id="app">
    <article class="content">
      <h1 class="content__title">To Do Manager</h1>
      <TextButton :action="'add'" @add-todo-event="addToDoItem" />
      <section v-if="todoEntries?.length" class="filters">
        <span class="filter__title">See Tasks:</span>
        <span class="filter__done" :class="{'filter__done-selected': doneFilter}" @click="filterByDone(true)">Done</span>
        <span class="filter__undone" :class="{'filter__undone-selected': undoneFilter}" @click="filterByDone(false)">Undone</span>
        <span class="filter__all" :class="{'filter__all-selected': !doneFilter && !undoneFilter}" @click="deleteFilters">All</span>
      </section>
      <section v-if="todoEntries?.length" class="filters filter__text">
        <span class="filter__title">Filter by content: <TextButton :action="'filter'" @filterTodoEvent="filterByText" /></span>
      </section>
    </article>
    <br/>
    <article class="content__list" v-if="todoEntries?.length">
      <h1 class="content__title">To Do List</h1>
      <ToDoList
        :todoEntries="todoFiltered"
        @item-complete-event="completeToDo"
        @delete-todo-event="deleteToDoItem"
        @edit-todo-event="updateToDoItem" />
    </article>
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue';
import TextButton from './components/TextButton.vue';
export default {
  name: 'App',
  components: {
    TextButton,
    ToDoList
  },
  data() {
    return {
      todoEntries: [],
      todoFiltered: [],
      doneFilter: false,
      undoneFilter: false,
      textFilter: false,
      textToFilterBy: ''
    }
  },
  methods: {
    equalsArrays() {
      this.todoFiltered = this.todoEntries;
    },
    addToDoItem(title) {
      let id;
      if (this.todoEntries?.length)
        id = Math.max.apply(Math, this.todoEntries.map((obj) => {return obj.id;})) + 1;
      else
        id = 1;
      const newToDoItem = {
        id: id,
        title: title,
        completed: false
      };
      this.todoEntries = [...this.todoEntries, newToDoItem];
      if (this.doneFilter)
        this.filterByDone(true);
      if (this.undoneFilter)
        this.filterByDone(false);
      if (this.textFilter)
        this.filterByText(this.textToFilterBy);
      if (!this.doneFilter && !this.undoneFilter && !this.textFilter)
        this.equalsArrays();
    },

    completeToDo(toDoId) {
      this.todoEntries.forEach(todo => {
        if (todo.id === toDoId)
          todo.completed = !todo.completed;
      });
      this.equalsArrays();
    },
    deleteFilters() {
      this.doneFilter = this.undoneFilter = false;
      this.equalsArrays()
      if (this.textFilter)
        this.todoFiltered = this.todoEntries.filter(todo => todo.title.toLowerCase().indexOf(this.textToFilterBy.toLowerCase()) > -1);
    },
    deleteToDoItem(toDoId){
      this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId);
      this.equalsArrays();
    },
    filterByDone(value) {
      if (value) {
        this.doneFilter = true;
        this.undoneFilter = false;
      } else {
        this.doneFilter = false;
        this.undoneFilter = true;
      }
      this.todoFiltered = this.todoEntries.filter(todo => todo.completed === value);
      if (this.textFilter)
        this.todoFiltered = this.todoFiltered.filter(todo => todo.title.toLowerCase().indexOf(this.textToFilterBy.toLowerCase()) > -1);
    },

    filterByText(text) {
      if (text) {
        this.todoFiltered = this.todoEntries.filter(todo => todo.title.toLowerCase().indexOf(text.toLowerCase()) > -1);
        this.textToFilterBy = text;
        this.textFilter = true;
      } else {
        this.equalsArrays();
        this.textToFilterBy = '';
        this.textFilter = false;
      }
      if (this.doneFilter)
        this.todoFiltered = this.todoFiltered.filter(todo => todo.completed);
      if (this.undoneFilter)
        this.todoFiltered = this.todoFiltered.filter(todo => !todo.completed);
    },

    updateToDoItem(data) {
      this.todoEntries.forEach(todo => {
        if (todo.id === data.id)
          todo.title = data.value;
      });
      this.equalsArrays();
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
  h1 {
    color: white;
  }
  
  span {
    padding: 0 0.5rem;
    font-size: 0.8rem;
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

  .filter__all {
    color: white;
    cursor: pointer;
  }

  .filter__all-selected {
    border: 1px solid white;
  }

  .filter__done {
    color: greenyellow;
    cursor: pointer;
  }

  .filter__done-selected {
    border: 1px solid greenyellow;
  }

  .filters {
    display: inline;
  }

  .filter__text {
    margin-left: 2rem;
  }

  .filter__title {
    color: lavender;
  }

  .filter__undone {
    color: palevioletred;
    cursor: pointer;
  }

  .filter__undone-selected {
    border: 1px solid palevioletred;
  }
</style>
