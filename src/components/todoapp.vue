<template>
    <div class="main">
        <h1>To do List</h1>
        <h2>{{ date }}</h2>
        <form @submit.prevent="addTodo">
            <input class="newTodo" v-model="newTodo" type="text" name="todo" placeholder="Add task here" maxlength="25">
            <button class="add" type="submit">Add</button>
        </form>
        <ul>
            <li class="li" v-for="(todo, index) in todos" :key="index">
                <div class="li_left">
                    <input class="check" v-model="todo.completed" type="checkbox" @change="toggleCompleted(todo)">
                    <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
                </div>
                <div class="li_right">
                    <button class="delete" v-if="!todo.completed" @click="deleteTodo(index)">Delete</button>
                    <button class="edit" v-if="!todo.completed" @click="editTodo(index, todo)">Edit</button>
                </div>
            </li>
        </ul>
        <button class="clear" @click="clearTodos">Clear all</button>
    </div>
</template>
  
<script>
    export default {
        data() {
            return {
                todos: [],
                newTodo: ''
            }
        },
        mounted() {
            this.todos = JSON.parse(localStorage.getItem('todos')) || []
        },
        watch: {
            todos: {
                handler(newTodos) {
                    localStorage.setItem('todos', JSON.stringify(newTodos))
                },
            deep: true
            }
        },
        methods: {
            addTodo() {
                this.todos.push({
                    text: this.newTodo,
                    completed: false
                })
                this.newTodo = ''
            },
            deleteTodo(index) {
                this.todos.splice(index, 1)
            },
            toggleCompleted(todo) {
                todo.completed = true
            },
            editTodo(index, todo) {
                this.todos.splice(index, 1, {
                    text: prompt('New task :', todo.text),
                    completed: todo.completed
                })
            },
            clearTodos() {
                this.todos = []
                localStorage.removeItem('todos')
            }
        },
        computed: {
            date() {
                const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                return new Date().toLocaleDateString('en-EN', options);
            }
        }
    }
</script>
  
<style>
    .main{
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    .newTodo{
        height: 30px;
        border-radius: 10px;
        border: solid 2px grey;
        padding-left: 10px;
    }

    .add {
        height: 36px;
        width: 70px;
        border-radius: 10px;
        border: none;
        margin-left: 10px;
        background-color: rgb(59, 207, 0);
    }

    button {
        font-size: 15px;
        font-weight: 600;
        color: black;
    }

    ul {
        list-style-type: none;
    }

    li {
        padding: 8px;
    }

    .li {
        display: flex;
        justify-content: space-between;
    }

    .li_left {
        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
    }

    .li_right {
        display: flex;
        justify-content: flex-end;
    }

    .check {
        transform: scale(2);
        margin-right: 10px;
    }

    .delete {
        height: 36px;
        width: 70px;
        border-radius: 10px;
        border: none;
        background-color: rgb(255, 208, 0);
        margin-right: 10px;
    }

    .edit {
        height: 36px;
        width: 70px;
        border-radius: 10px;
        border: none;
        background-color: rgb(69, 131, 189);
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .clear{
        height: 36px;
        width: 100px;
        border-radius: 10px;
        border: none;
        background-color: rgb(255, 0, 0);
        margin-right: 10px;
    }
</style>
  