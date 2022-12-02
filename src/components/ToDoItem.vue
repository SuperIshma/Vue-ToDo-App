<template>
    <div :class="{ 'test': !edit }">
        <p v-if="!edit" :class="{ 'completed' : todoItem.completed }" @click="markComplete" class="todoItem">{{ todoItem.title }}</p>
        <div v-if="edit">
            <form class="edit__form" @submit.prevent="updateToDo">
                <input type="text" class="search__input" v-model="task" name="task">
                <button type="submit" class="search__submit">Update</button>
            </form>
        </div>
        <div>
            <button v-if="!edit" title="Edit" class="btn edit" @click="editToDo"><i class="fa fa-edit"></i></button>
            <button v-if="!edit" title="Delete" class="btn delete" @click="$emit('delete-todo-event', todoItem.id)"><i class="fa fa-trash"></i></button>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'ToDoItem',
    components: {
    },
    data() {
    return {
      edit: false,
      task: '',
    }
  },
    props: [
      "todoItem"
    ],
    methods: {
        editToDo() {
            this.edit = true;
            this.task = this.todoItem.title;
        },
        markComplete() {
            // eslint-disable-next-line vue/no-mutating-props
            this.todoItem.completed = !this.todoItem.completed;
        },
        updateToDo() {
            const data = {id: this.todoItem.id, value: this.task};
            this.$emit('editTodoEvent', data);
            this.edit = false;
            this.task = '';
        }
    }
  }
</script>

<style>
    .btn {
        border: none;
        color: white;
        padding: 12px 16px;
        font-size: 16px;
        cursor: pointer;
    }
    .delete {
        background-color: #fd658c;
    }
    .edit {
        background-color: blue;
    }
    .test {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .completed {
        text-decoration: line-through;
    }
    .todoItem {
        cursor: pointer;
    }
    .edit__form {
        display: flex;
        margin: 0;
    }
    .search__input {
        flex: 1;
        font-size: 1rem;
    }
    .search__submit {
        font-size: 1rem;
    }
    form {
        margin: 0;
    }
</style>