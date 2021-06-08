<template>
    <div class="newTodo">
        <input v-model="newTodo" @keypress.enter="addTodo" type="text" placeholder="Enter your task to do..." v-focus> 
        <button @click="addTodo" class="green"> 
            <span class="button-name">Add</span>
            <span class="fa fa-plus-circle"></span>
        </button>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                newTodo: '',
            }
        },
        methods: {
            addTodo(){
                if ((this.newTodo.length > 0) && (this.newTodo.length < 35)){
                    this.$emit('add-todo', this.newTodo)
                } else alert('Length must be > 0 and < 35')
                this.newTodo = ''
            }
        }
    }
</script>

<style lang="scss">
    .newTodo{
        display: flex;
        justify-content: center;
        align-items: center;
    }
    input{
        width: 325px;
        height: 30px;
        border: 1px solid #ced4da;
        border-radius: 7px;
        outline: none;
        font-family: 'Noto Sans HK', sans-serif;
        padding-left: 10px;
        font-size: 16px;
    
        &:focus{
            box-shadow: 0 0 0 2px #88b8ff;
            border: 1px solid #5b89ce;
        }
    }
    button{
        margin: 5px;
        padding: 10px;
        background-color: transparent;
        border: none;
        cursor: pointer;
    }
    .green{
        color: #28a745;
        font-size: 16px;
    
        &:hover{
            color: #fff;
            background-color: #28a745;
            border-radius: 7px;
        }
    }
    .button-name{
        padding-right: 5px;
    }
    @media screen and (max-width: 500px) {
        .button-name{
            display: none;
        }
    }
</style>