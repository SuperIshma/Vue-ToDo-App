<template>
    <div :class="{'filter': action==='filter'}">
      <form :class="formClass" @submit.prevent="sendToDo">
        <input v-if="!isFilter" type="text" class="text__input text__input-s" v-model="title" name="title" placeholder="Enter the task to do...">
        <input v-if="isFilter" type="text" class="text__filter" v-model="title" name="title" placeholder="Enter the task to find...">
        <button type="submit" :class="buttonClass">{{buttonText}}</button>
      </form>
    </div>
</template>

<script>
    export default {
        name: 'TextButton',
        computed: {
            buttonClass() {
                return {
                    'text__submit': this.action !== 'filter',
                    'text__submit-filter': this.action === 'filter'
                }
            },
            buttonText() {
                return this.action === 'add' ? 'Add' : (this.action === 'edit' ? 'Update' : 'Filter');
            },
            formClass() {
                return {
                    'input': this.action === 'add',
                    'edit': this.action === 'edit',
                    'filter': this.action === 'filter'
                }
            },
            isFilter() {
                return this.action === 'filter';
            }
        },
        created() {
            if (this.task) this.title = this.task;
        },
        data() {
            return {
                title: ''
            }
        },
        methods: {
            sendToDo(){
                switch (this.action) {
                    case 'add':
                        this.$emit("add-todo-event", this.title);
                        break;
                    case 'edit':
                        this.$emit('updateTodoEvent', this.title);
                        break;
                    case 'filter':
                        this.$emit('filterTodoEvent', this.title);
                }
                if (this.action !== 'filter')
                    this.title = ''
            }
        },
        props: [
            "action",
            "task"
        ]
}
</script>
<style>
    .edit {
        display: flex;
    }
    .filter {
        display: inline;
    }
    .input {
        display: flex;
        margin-bottom: 2.5rem;
    }
    .text__filter {
        flex: 1;
        padding: 0.2rem;
        font-size: 0.8rem;
    }
    .text__input {
        flex: 1;
        font-size: 1rem;
    }
    .text__submit {
        font-size: 1rem;
    }
    .text__submit-filter {
        padding: 0.2rem;
        font-size: 0.8rem;
    }
    @media screen and (max-width: 599px) {
        .text__input-s {
            padding: 0.3rem;
        }
    }

    @media screen and (min-width: 600px) {
        .text__input-s {
            padding: 1rem;
        }
    }
</style>