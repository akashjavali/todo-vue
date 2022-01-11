<template>
    <div class="todo_wrap text-center mt-5">
        <div class="container">
            <h2>Todo App</h2>
            <form @submit.prevent="onSubmit">
                <div class="d-flex">
                    <input
                        type="text"
                        class="form-control"
                        placeholder="Add todos"
                        v-model="todoInput"
                    />
                    <button class="btn btn-warning">
                        {{ isEdit ? 'Edit Todos' : 'Add Todos' }}
                    </button>
                </div>
            </form>

            <div class="todo_list_wrap">
                <ul>
                    <li
                        v-for="(todo, index) in todos"
                        :key="todo.id"
                        class="todo_list"
                    >
                        <h5
                            @click="toggleDone(todo)"
                            :class="{ done: todo.done }"
                            class="todo_list_text"
                        >
                            {{ todo.todo }}
                        </h5>
                        <button
                            class="btn btn-danger mx-2"
                            @click="onDelete(index)"
                        >
                            Delete
                        </button>
                        <button
                            class="btn btn-success"
                            @click="onEdit(index, $event)"
                        >
                            Edit
                        </button>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, reactive } from 'vue';
import { v4 as uuidv4 } from 'uuid';
export default {
    setup() {
        const getTodos = localStorage.getItem('todoList')
        const setTodos = JSON.parse(getTodos)
        const todoInput = ref('');
        const todoEdit = ref(null);
        const isEdit = ref(false);
        const todos = reactive(setTodos);
        const setLocalData = () => {
            return localStorage.setItem('todoList', JSON.stringify(todos))
        }
        const onSubmit = () => {
            if (todoEdit.value === null) {
                todos.push({
                    id: uuidv4(),
                    done: false,
                    todo: todoInput.value,
                });
                setLocalData()
            } else {
                console.log('came to else block');
                todos[todoEdit.value].todo = todoInput.value;
                todoEdit.value = null;
                setLocalData()
                isEdit.value = false
            }

            todoInput.value = '';
        };
        const toggleDone = (todo) => {
            todo.done = !todo.done;
            setLocalData()
        };
        const onDelete = (index) => {
            todos.splice(index, 1);
            setLocalData()
        };
        const onEdit = (index) => {
            todoInput.value = todos[index].todo;
            todoEdit.value = index;
            isEdit.value = true
        };
        return {
            todoInput,
            todos,
            onSubmit,
            toggleDone,
            onDelete,
            onEdit,
            isEdit
        };
    },
    components: {},
};
</script>

<style scoped>
.todo_list {
    text-align: left;
    margin-top: 10px;
    display: flex;
    
}
.todo_list_text {
    display: flex;
    align-items: center;
    margin-bottom: 0;
    margin-right: 10px;
}
.done {
    text-decoration: line-through;
}


</style>
