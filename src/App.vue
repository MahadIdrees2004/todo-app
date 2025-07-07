<script setup>
import { ref } from 'vue'
import Head from './components/head.vue'
import Todo from './components/todo.vue'
import Foot from './components/foot.vue'
// const t=ref([
//   { id: 1, title: 'Learn Vue.js',description: "I go to learn vue on website", completed: false },
//   { id: 2, title: 'Build a Todo App',description: "I go to build my todo app", completed: false },
//   { id: 3, title: 'Deploy the App',description: "I go to deploy app", completed: false }
// ])
const t = ref([])
const storedTasks = localStorage.getItem('tasks')
t.value = JSON.parse(storedTasks)
if (!t.value) {
  t.value = [];
}
const deltask = (item) => {
  t.value.splice(t.value.indexOf(item), 1);
  localStorage.setItem('tasks', JSON.stringify(t.value))
  alert("Task Deleted Successfully")
}  
const temp=ref({
  title: '',
  description: '',
  completed: ''
})
const clearAll = () => {
  t.value = [];
  localStorage.removeItem('tasks');
}
const updindex= ref(-1)
const alpha= ref(false)
const a= ref(false)
const add = () => {
    alert("Task Added Successfully")
    alpha.value = false;
    t.value.push({
       id: t.value.length + 1,
       title: temp.value.title,
       description: temp.value.description,
       completed: temp.value.completed
     });
  localStorage.setItem('tasks', JSON.stringify(t.value))
       temp.value = { title: '', description: '', completed: false }
  }
const updtask = (item) =>
{
  a.value = true
  updindex.value = t.value.indexOf(item)
  temp.value = t.value[updindex.value]
}
const updaterecord = () => {
  t.value[updindex.value].title = temp.value.title
  t.value[updindex.value].description = temp.value.description
  t.value[updindex.value].completed = temp.value.completed
  a.value = false
  localStorage.setItem('tasks', JSON.stringify(t.value))
  alert("Task Updated Successfully")
}
</script>
<template>
  <Head/>
   <form v-if="a" class="form" @submit.prevent="updaterecord()">
    <h1 class="centr">Update Task</h1>  
  <div class="form-group" >
    <label for="title">Title</label>
    <input type="title" class="form-control" v-model="temp.title" id="title" placeholder="Enter title" required/>
  </div> 
  <div class="form-group" >
    <label for="desc">Description</label>
    <input type="desc" class="form-control" v-model="temp.description" id="desc" placeholder="Enter Description" required/>
  </div>
  <div class="form-group">
    <label for="status">Status</label>
    <select class="form-control" v-model="temp.completed" vid="status" required>
      <option :value="false">Pending</option>
      <option :value="true">Completed</option>
    </select>
  </div>
  <button type="submit"style="margin:20px ;width:200px; " class="btn btn-primary centr">Add</button>
</form>
  <form v-if="alpha" class="form" @submit.prevent="add()">
  <div class="form-group" >
    <h1 class="centr">Add Task</h1>
    <label for="title">Title</label>
    <input type="title" class="form-control" v-model="temp.title" id="title" placeholder="Enter title" required/>
  </div> 
  <div class="form-group" >
    <label for="desc">Description</label>
    <input type="desc" class="form-control" v-model="temp.description" id="desc" placeholder="Enter Description" required/>
  </div>
  <div class="form-group">
    <label for="status">Status</label>
    <select class="form-control" v-model="temp.completed" vid="status" required>
      <option :value="false">Pending</option>
      <option :value="true">Completed</option>
    </select>
  </div>
  <button type="submit"  style="margin:20px auto;width:200px;" class="btn btn-primary centr">Add</button>
</form>
  <Todo v-if="!alpha & !a" :todo="t" @delete-task="deltask" @update-task="updtask"/>
  <button v-if="!alpha & !a" type="button" class="task" @click="alpha=true">Add Task</button>
  <button v-if="!alpha & !a" type="button" class="clear" @click="clearAll">Clear All</button>
  <Foot/>
</template>

<style scoped>

body {
  background-color: #f8f9fa;
}
.Head {
  background-color: #343a40;
  color: #ffffff;
  padding: 20px;    
  text-align: center;
}
.task
{
  margin: 20px auto;
  width : 300px;
  display: block;
  background-color: #007bff;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1.2em;
  cursor: pointer;
}
.form {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin: 20px auto;
  max-width: 400px;
}
.clear {
  margin: 20px auto;
  width : 300px;
  display: block;
  background-color: #dc3545;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1.2em;
  cursor: pointer;
}
.form-group {
  margin-bottom: 15px;
}
.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  font-size: 1em;
}
.form-control:focus {
  border-color: #007bff;
  box-shadow: 0 0 0 0.2rem rgba(0,      123, 255, 0.25);
}
.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
  color: #ffffff;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1.2em;
  cursor: pointer;
}
.btn-primary:hover {
  background-color: #0056b3;
  border-color: #004085;
}
.btn-primary:focus {
  box-shadow: 0 0 0 0.2rem rgba(0,
  123, 255, 0.25);
}
.centr {
  text-align: center;
  margin: 20px auto;
}
</style>
