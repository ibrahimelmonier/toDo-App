<template>
    <div>
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <toDoItem v-for="(todo) in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!leftTodo">
            </toDoItem>
        </transition-group>

        <div class="extra">
            <div>
                <label>
                    <input type="checkbox" :checked="!leftTodo" @change="checkAllTodos">
                    Check All
                </label>
            </div>
            <div>
                {{ left }} items left
            </div>
        </div>

        <div class="extra">
            <div>
                <button :class="{active : filter == 'all'}" @click="filter = 'all'">All</button>
                <button :class="{active : filter == 'active'}" @click="filter = 'active'">Active</button>
                <button :class="{active : filter == 'completed'}" @click="filter = 'completed'">Completed</button>
            </div>

            <div>
                <transition name="fade">
                    <button v-if="showClear" @click="clearCompleted">Clear Completed</button>
                </transition>
            </div>

        </div>



    </div>
</template>

<script>
    import ToDoItem from './toDoItem.vue';

    export default {
        name: 'ToDoList',
        components: {
            ToDoItem,
        },
        // props: {
        //     msg: String
        // },
        data() {
            return {
                newTodo: '',
                idForTodo: 5,
                beforeEdit: '',
                filter: 'all',
                todos: [
                    {
                        'id': 1,
                        'title': 'Learn Vue',
                        'completed': false,
                        'editing': false
                    },{
                        'id': 2,
                        'title': 'Learn Vue with Laravel',
                        'completed': false,
                        'editing': false
                    },
                ]
            }
        },
        created() {
            eventBus.$on('removedTodo', (id) => this.removeTodo(id));
            eventBus.$on('finishedEdit', (data) => this.finishedEdit(data));
        },
        computed: {
            left() {
                return this.todos.filter(todo => !todo.completed).length
            },
            leftTodo() {
                return  this.left != 0
            },
            todosFiltered() {
                if(this.filter == 'all') {
                   return this.todos
                } else if (this.filter == 'active') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter == 'completed') {
                    return this.todos.filter(todo => todo.completed)
                }
            },
            showClear() {
                return this.todos.filter(todo => todo.completed).length > 0
            }
        },
        methods: {
            addTodo() {
                if (this.newTodo.trim().length == 0) {
                    return
                }
                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false
                })
                this.newTodo = ''
                this.idForTodo++
            },
            removeTodo(id) {
                const index = this.todos.findIndex((item) => item.id == id);
                this.todos.splice(index, 1)
            },
            checkAllTodos() {
                this.todos.forEach((todo) => todo.completed = event.target.checked)
            },
            clearCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed)
            },
            finishedEdit(data) {
                const index = this.todos.findIndex((item) => item.id == data.id);
                this.todos.splice(index, 1, data)
            }
        }
    }
</script>

<style lang="scss">
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");
    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        &:focus {
            outline: 0;
            border: 1px solid #42b983;
        }
    }
    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.3s;
    }
    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: black;
        }
    }
    .todo-item-left {
        display: flex;
        align-items: center;
    }
    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }
    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        &:focus {
            outline: none;
            border: 1px solid #42b983;
        }
    }
    .completed {
        text-decoration: line-through;
        color: #42b983;
    }
    .extra {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid #42b983;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    button {
        color: inherit;
        margin: 3px;
        padding: 3px 7px;
        text-decoration: none;
        border: 1px solid transparent;
        border-radius: 3px;
        &:hover {
            background-color: #42b983;
            color: #fff;
        }
        &:focus {
            outline: none;
            color: #ffffff;
        }
    }
    .active {
        background-color: #42b983;
    }
    .fade-enter-active, .fade-leave-action {
        transition: opacity .2s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>
