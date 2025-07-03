<template>
    <div class="Todo" >
        <h2>Todo List</h2>
        <ul>
            <p v-if="todo.length==1">No Task Pending</p>
            <li v-for="item in todo">
                <h4>{{ item.title }}</h4>
                <p>{{ item.description }}</p>
                <p style="color: blue;">Status: {{ item.completed ? 'Completed' : 'Pending' }}</p>
                <div v-if="!item.completed">
                <button class="status" @click="item.completed=!item.completed">Mark as Completed</button>
                </div>
                <div v-else="item.completed">
                     <button class="status" @click="item.completed=!item.completed">Mark as Pending</button>
                </div>
                <button class="del" @click="del(item)">Delete</button>
                <button class="upd" @click="updat(item)">Update</button>
            </li>
        
        </ul>
        <p v-if="todo.length === 0">No todos available.</p>
        </div>
</template>
<script setup>
const emit = defineEmits(['delete-task'])
import { ref } from 'vue';
const i = ref(0);
const updat = (item) => {
    emit('update-task', item);
};
const del = (item) => {
    emit('delete-task', item);
};
defineProps(
    {
        todo:
        {
            type: Array,
            required: true
        }
    }
    
)
    </script>
    <style scoped>
.Todo {
    
    background-color: #f8f9fa;
    padding: 20px;
    border-radius: 50px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 40px auto;
    max-width: 600px;
}
.Todo h2 {
    color: #343a40;
}
.Todo ul {
    list-style-type: none;
    padding: 0;
}
.Todo li {
    margin-bottom: 15px;
    padding: 10px;
    border: 1px solid #dee2e6;
    border-radius: 5px;
    background-color: #ffffff;
}
.Todo li h4 {
    margin: 0;
    color: #495057;
}
.Todo li p {
    margin: 5px 0;
}
.Todo li p:last-child {
    font-weight: bold;
}
.Todo p {
    color: #6c757d;
}
.Todo p:last-child {
    color: #28a745;
}
.Todo p:empty {
    display: none;
}
.Todo p:empty::before {
    content: "No description available.";
    color: #6c757d;
}
.Todo button {
    margin-right: 10px;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
.Todo button.status {
    background-color: #28a745;
    color: white;
    margin-bottom: 10px;
}
.Todo button.del {
    background-color: #dc3545;
    color: white;
}
.Todo button.upd {
    background-color: #007bff;
    color: white;

}
</style>
