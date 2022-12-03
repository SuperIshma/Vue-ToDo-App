<template>
  <div id="app">
    <article class="content-media content">
      <h1 class="content__title">To Do Manager</h1>
      <TextButton :action="'add'" @add-todo-event="addToDoItem" />
      <section v-if="todoEntries?.length" class="filters">
        <span class="filter__title">See Tasks:</span>
        <span class="filter__done" :class="filterDoneClass" @click="filterByDone(true)">Done</span>
        <span class="filter__undone" :class="filterUndoneClass" @click="filterByDone(false)">Undone</span>
        <span class="filter__all" :class="noFilterClass" @click="deleteFilters">All</span>
      </section>
      <section v-if="todoEntries?.length" class="filters filter__text">
        <span class="filter__title">Filter by content: <TextButton :action="'filter'" @filterTodoEvent="filterByText" /></span>
      </section>
    </article>
    <br/>
    <article class="content__list content__list-media" v-if="todoEntries?.length">
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
  computed: {
    filterDoneClass() {
      return {'filter__done-selected': this.doneFilter}
    },
    filterUndoneClass() {
      return {'filter__undone-selected': this.undoneFilter}
    },
    noFilterClass() {
      return {'filter__all-selected': !this.doneFilter && !this.undoneFilter}
    }
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
      this.filterValues();
    },

    completeToDo(toDoId) {
      this.todoEntries.forEach(todo => {
        if (todo.id === toDoId)
          todo.completed = !todo.completed;
      });
    },

    deleteFilters() {
      this.doneFilter = this.undoneFilter = false;
      this.filterValues();
    },

    deleteToDoItem(toDoId){
      this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId);
      this.equalsArrays();
    },

    filterValues() {
      this.equalsArrays();
      if (this.doneFilter || this.undoneFilter)
        this.todoFiltered = this.todoFiltered.filter(todo => todo.completed === this.doneFilter);
      if (this.textFilter && this.textToFilterBy)
        this.todoFiltered = this.todoFiltered.filter(todo => todo.title.toLowerCase().indexOf(this.textToFilterBy.toLowerCase()) > -1);
    },

    filterByDone(value) {
      this.doneFilter = value;
      this.undoneFilter = !this.doneFilter;
      this.filterValues();
    },

    filterByText(text) {
      if (text) {  
        this.textToFilterBy = text;
        this.textFilter = true;
      } else {
        this.textToFilterBy = '';
        this.textFilter = false;
      }
      this.filterValues();
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
  h1 {
    color: white;
  }
  
  span {
    padding: 0 0.5rem;
    font-size: 0.8rem;
  }

  .content {
    background-color: #4a58b2;
    box-shadow: 2px 2px 3px gray;
    border-radius: 1rem;
  }

  .content__list {
    background-color: #a879fa;
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

  @media screen and (max-width: 599px) {
    .content-media {
      width: 100%;
      font-size: 1rem;
      padding: 1rem
    }

    .content__list-media {
      width: 100%;
      font-size: 1rem;
      padding: 1rem;
    }

    .filters {
      display: block;
    }

    .filter__text {
      margin-left: 0;
    }
  }

  @media screen and (min-width: 600px) {
    .content-media {
      width: 768px;
      max-width: 768px;
      padding: 3rem;
    }

    .content__list-media {
      width: 768px;
      max-width: 768px;
      padding: 3rem;
    }

    .filters {
      display: inline;
    }

    .filter__text {
      margin-left: 2rem;
    }
  }
</style>
