<template>
    <div>
        <ul>
          <li :key="item.id" v-for="item in todoEntries">
              <ToDoItem
                :todoItem="item"
                @delete-todo-event="$emit('delete-todo-event', item.id)"
                @edit-todo-event="(data) => $emit('editTodoEvent', data)" />
          </li>   
      </ul>
    </div>
</template>

<script>
import ToDoItem from "./ToDoItem";

export default {
  name: 'ToDoList',
  components: {
      ToDoItem
  },
  props: [
      "todoEntries"
  ]
}
</script>
<style>
ul li {
    border: 2px solid hsla(0,0%,100%,.35);
    padding: 12px 24px;
    border-radius: 6px;
    margin-bottom: 12px
}
li {
    list-style-type: none;
}
</style>