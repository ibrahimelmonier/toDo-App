<template>
    <div class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" v-model="completed" @change="doneEdit">
            <div v-if="!editing" @dblclick="editTodo" class="todo-item-label" :class="{ completed : completed }">{{ title }}</div>
            <input v-else class="todo-item-edit" type="text" v-model="title" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus>
        </div>
        <div class="remove-item" @click="removeTodo(id)">
            &times;
        </div>
    </div>
</template>

<script>
    export default {
        name: "toDoItem",
        props: {
            todo: {
                type: Object,
                required: true,
            },
            checkAll: {
                type: Boolean,
                required: true,
            }
        },
        data() {
            return {
                'id': this.todo.id,
                'title': this.todo.title,
                'completed': this.todo.completed,
                'editing': this.todo.editing,
                'beforeEdit': '',
            }
        },
        watch: {
           checkAll() {
               if(this.checkAll) {
                   this.completed = true
               }else {
                   this.completed = this.todo.completed
               }
            }
        },
        directives: {
            focus: {
                // directive definition
                inserted: function (el) {
                    el.focus()
                }
            }
        },
        methods: {
            removeTodo(id) {
                eventBus.$emit('removedTodo', id);
            },
            editTodo() {
                this.beforeEdit = this.title;
                this.editing = true
            },
            doneEdit() {
                if (this.title.trim().length == 0 && this.title.trim().length == '') {
                    this.title = this.beforeEdit
                }
                this.editing = false;
                eventBus.$emit('finishedEdit', {
                    'id': this.id,
                    'title': this.title,
                    'completed': this.completed,
                    'editing': this.editing,
                });
            },
            cancelEdit() {
                this.title = this.beforeEdit;
                this.editing = false
            },
        }
    }
</script>
