<template>
    <div>
      <form class="search" @submit.prevent="addToDo">
        <input type="text" class="search__input" v-model="title" name="title" placeholder="Enter the task to do...">
        <button type="submit" class="search__submit">Add</button>
      </form>
    </div>
</template>
<script>
    export default {
        name: 'AddToDoButton',
        data() {
            return {
                title: ''
            }
        },
        methods: {
            addToDo(){
                const newToDoObject = {
                    id: 0,
                    title: this.title,
                    completed: false
                }
                
                this.$emit("add-todo-event", newToDoObject);
                this.title= '';
            }
        }
}
</script>
<style>
    .search {
        display: flex;
        margin-bottom: 2.5rem;
    }
    .search__input {
        flex: 1;
        padding: 1rem;
        font-size: 1rem;
    }
    .search__submit {
        font-size: 1rem;
    }
</style>