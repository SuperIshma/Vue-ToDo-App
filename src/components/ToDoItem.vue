<template>
    <div :class="{ 'container': !edit }">
        <p v-if="!edit" :class="{ 'completed' : todoItem.completed }" @click="markComplete" class="todoItem">{{todoItem.title}}</p>
        <div v-if="edit">
            <TextButton :action="'edit'" :task="todoItem.title" @update-todo-event="updateToDo" />
        </div>
        <div>
            <button v-if="canEdit" title="Edit" class="btn btn-s edit" @click="editToDo"><i class="fa fa-edit"></i></button>
            <button v-if="!edit" title="Delete" class="btn btn-s delete" @click="$emit('delete-todo-event', todoItem.id)"><i class="fa fa-trash"></i></button>
        </div>
    </div>
</template>

<script>
  import TextButton from './TextButton.vue';
  export default {
    name: 'ToDoItem',
    components: {
        TextButton
    },
    computed: {
        canEdit() {
            return !this.edit && !this.todoItem.completed
        }
    },
    data() {
        return {
        edit: false,
        task: '',
        }
    },
    methods: {
        editToDo() {
            this.edit = true;
            this.task = this.todoItem.title;
        },
        markComplete() {
            this.$emit('itemCompleteEvent', this.todoItem.id);
        },
        updateToDo(task) {
            const data = {id: this.todoItem.id, value: task};
            this.$emit('editTodoEvent', data);
            this.edit = false;
            this.task = '';
        }
    },
    props: [
      "todoItem"
    ]
  }
</script>

<style>
    .btn {
        border: none;
        color: white;
        cursor: pointer;
    }

    @media screen and (max-width: 599px) {
        .btn-s {
            padding: 3px 4px;
            font-size: 12px;
        }
    }
    @media screen and (min-width: 600px) {
        .btn-s {
            padding: 12px 16px;
            font-size: 16px;
        }
    }

    .completed {
        text-decoration: line-through;
    }
    .container {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .delete {
        background-color: #fd658c;
    }
    .edit {
        background-color: blue;
    }
    .edit__form {
        display: flex;
        margin: 0;
    }
    .todoItem {
        cursor: pointer;
    }
    
</style>