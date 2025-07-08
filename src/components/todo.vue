<template>
    <div class="Todo" >
        <h2>Todo List</h2>
        <p v-if="todo.length==1">No Task Pending</p>
        <ul>
            <li v-for="item in todo">
                <table class="outer">
                    <tr>
                        <td>
                            <table class="inner">
                                <tr>
                                    <td><strong>Title:&nbsp</strong>
                                        {{ item.title }}</td>
                                    <td>
                                    <p style="color: blue;">Status: {{ item.completed ? 'Completed' : 'Pending' }}</p>
                                    </td>
                                </tr>
                                <tr>
                                    <td><strong>Description:&nbsp</strong>{{ item.description }}</td>
                                </tr>
                            </table>
                        </td>

                        <td style="text-align: right; padding-right: 20px; height: 100%;">
                            <button id="123" v-if="item.completed" class="status" @click="statusfunc(item)">Mark as Pending</button>
                            <button id="123" v-else class="status" @click="statusfunc(item)">Mark as Completed</button>
                            <button id="123" class="del" @click="del(item)">Delete</button>
                            <button id="123" class="upd" @click="updat(item)">Update</button>
                        </td>
                    </tr>
                </table>
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
const statusfunc = (item) => {
    emit('update-status', item);
    
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
.inner td:first-child {
    width: 250px;
    min-width: 250px;
    max-width: 250px;
    vertical-align: top;
    word-break: break-word;
}
.inner td:nth-child(2) {
    width: 180px;
    min-width: 180px;
    max-width: 180px;
    vertical-align: top;
    word-break: break-word;
}
.Todo {
    width: 100%;
    height: 100%;
    padding: 20px;
    background-color: #f0f0f0;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    background-color: #ffffff;
    border: 1px solid #dddddd;
    margin: 10px 0;
    padding: 5px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.outer {
    width: 100%;
    height: 100%;
    padding: 20px;
}
.inner {
    width: 100%;
    height: 100%;
    padding: 20px;
}
.status {
    background-color: #4CAF50; /* Green */
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
}
.del {
    background-color: #f44336; /* Red */
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
}
.upd {
    background-color: #008CBA; /* Blue */
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
}
</style>

