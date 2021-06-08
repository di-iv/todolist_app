<template>
    <div class="todos">      
        <div class="amount" v-if="todos.length>0">
            <div 
                :class="{selected: filterName == ''}" 
                @click="filterTodo('all')"
            >
                All <span>{{todos.length}}</span>
            </div>
            <div 
                :class="{selected: filterName == 'false'}" 
                @click="filterTodo('false')"
            >
                Uncompleted <span>{{todos.length-countCompleted}}</span>
            </div>
            <div 
                :class="{selected: filterName == 'true'}" 
                @click="filterTodo('true')"
            >
                Completed <span>{{countCompleted}}</span>
            </div>
        </div>
        <div
            class="task" v-for="(todo, index) in filteredTodo" :key="index" 
            v-bind:class="{isDone:todo.completed}" 
        >
            <div 
                @click="toggleTodo(index)" 
                class="title"            
                v-if="!todo.editing"
            >
                <span>{{todo.description}}</span>
            </div>
            <div v-else class="title">
                <input type="text" 
                    v-model="newTodoDescription" 
                    v-focus @blur="finishEditing(index)" 
                    v-on:keypress.enter="finishEditing(index)"
                />
            </div>
            
            <button @click="startEditing(index)" class="primary">
                <span class="button-name">Edit</span>
                <span class="fa fa-edit"></span>
            </button>
            <button @click="deleteTodo(index)" class="danger">
                <span class="button-name">Delete</span>
                <span class="fa fa-trash"></span>
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            todos: Array,
        },
        data(){
            return{
                countCompleted: 0,
                newTodoDescription: '',
                filterName: ''
            }
        },
        mounted(){
            if (localStorage.countCompleted) this.countCompleted = localStorage.countCompleted
        },
        computed:{
            filteredTodo(){
                var compl = this.filterName;
                return this.todos.filter(function(todo){
                    if(compl==='') return true;
                    else return todo.completed.toString().indexOf(compl) > -1
                })
            }
        },
        methods: {
            deleteTodo(index){
                if (this.todos.[index].completed == true) this.countCompleted--;
                localStorage.countCompleted = this.countCompleted;
                this.$emit('delete-todo', index);
            },
            toggleTodo(index){
                this.$emit('on-toggle', index);
                this.todos.[index].completed == true ? this.countCompleted++ : this.countCompleted--;
                localStorage.countCompleted = this.countCompleted
            },
            startEditing(index){
                this.todos.[index].editing = true
                this.newTodoDescription = this.todos.[index].description
            },
            finishEditing(index){
                this.todos.[index].editing = false
                this.$emit('on-edit', index, this.newTodoDescription)
            },
            filterTodo(name){
                if (name == 'all') name = ''
                this.filterName = name
            }
        }
    }
</script>

<style lang="scss" scoped>
    .todos{
        margin-top: 10px;
    }
    .amount{
        display: flex;
        justify-content: flex-end;
        align-items: center;
        padding-bottom: 10px;
    
        div{
            margin-left: 20px;
            font-size: 11px;
            color: #999;
            cursor: pointer;

            span{
                padding: 2px 7px;
                border: 1px solid #999;
                border-radius: 7px;
            }
        }
        .selected{
            color: #000;
            span{
                border-color: #000;
            }
        }        
    }
    .task{
        display: flex;
        margin-top: 10px;
        padding: 5px;
        align-items: center;
        border: 1px solid #ced4da;
        border-radius: 7px;
    }
    .title{
        display: flex;
        flex-direction: row;
        flex-grow: 1;
        max-width: 319px;
        height: 34px;
        margin-left: 10px;
        cursor: pointer;
        span{
            padding-left: 10px;
            align-self: center;
            font-size: 16px;
        }
    }
    button{
        margin: 5px;
        padding: 10px;
        background-color: transparent;
        border: none;
    }
    .primary{
        color: #007bff;
        &:hover{
            color: #fff;
            background-color: #007bff;
            border-radius: 7px;
        }   
    }
    .danger{
        color: #dc3545;
        &:hover{
            color: #fff;
            background-color: #dc3545;
            border-radius: 7px;
        }
    }
    .isDone{
        opacity: 0.8;
        background-color: #e4e4e4;
        .title span{
            text-decoration: line-through;
        }
    }
    @media screen and (max-width: 550px) {
        #app{
            margin-top: 30px;
        }
        .amount div{
            font-size: 9px;
        }
        .title{
            margin-left: 0;  
            span{
                font-size: 12px;
            }
            input{
                width: 220px;
            } 
        }
        input{
            font-size: 12px;
        }
        button{
            margin: 0px;
            padding: 7px;
        }
    }
</style>