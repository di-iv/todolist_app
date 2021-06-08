<template>
    <div class="todoList">
        <h2>To-do list</h2>
        <TodoInput @add-todo="addTodo"/>
        <TodoItem 
            :todos="todos" 
            @delete-todo="deleteTodo" 
            @on-toggle="toggleTodo" 
            @on-edit="onEditing"
        />
    </div>
</template>

<script>
    import TodoItem from './TodoItem';
    import TodoInput from './TodoInput';

    export default {
        components: {
            TodoItem,
            TodoInput
        },
        data(){
            return{
            todos: [],
            test: ''
            }
        },
        mounted(){
            if (localStorage.todos) this.todos = JSON.parse(localStorage.todos)
        },
        methods: {
            updateStorage(){
                localStorage.todos = JSON.stringify(this.todos)
            },
            deleteTodo(index){
                this.todos.splice(index, 1);
                this.updateStorage()
            },
            addTodo(newTodo){
                this.todos.push({description: newTodo, completed: false, editing: false})
                this.updateStorage()
            },
            toggleTodo(index){
                this.todos.[index].completed = !this.todos.[index].completed
                this.updateStorage()
            },
            onEditing(index, newTodoDescription){
                if ((newTodoDescription.length > 0) && (newTodoDescription.length < 35)){
                    this.todos.[index].description = newTodoDescription
                    this.updateStorage()
                }
            }
        }
    }
</script>

<style>
    .todoList{
        max-width: 500px;
        margin: 0 auto;
    }
</style>