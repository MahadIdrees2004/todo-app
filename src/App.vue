<script setup>
import { ref } from 'vue'
import Head from './components/head.vue'
import Todo from './components/todo.vue'
import Foot from './components/foot.vue'
const t=ref([
  { id: 1, title: 'Learn Vue.js',description: "I go to learn vue on website", completed: false },
  { id: 2, title: 'Build a Todo App',description: "I go to build my todo app", completed: false },
  { id: 3, title: 'Deploy the App',description: "I go to deploy app", completed: false }
])
const deltask = (item) => {
  t.value.splice(t.value.indexOf(item), 1);
  alert("Task Deleted Successfully")
}  
const temp=ref({
  title: '',
  description: '',
  completed: ''
})
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
  }
const updtask = (item) =>
{
  a.value = true
  updindex.value = t.value.indexOf(item)
}
const updaterecord = () => {
  t.value[updindex.value].title = temp.value.title
  t.value[updindex.value].description = temp.value.description
  t.value[updindex.value].completed = temp.value.completed
  a.value = false
  alert("Task Updated Successfully")
}
</script>
<template>
  <Head/>
   <form v-if="a" class="form" @submit.prevent="updaterecord()">
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
  <button type="submit"  style="margin:20px auto;width:200px;" class="btn btn-primary">Add</button>
</form>
  <form v-if="alpha" class="form" @submit.prevent="add()">
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
  <button type="submit"  style="margin:20px auto;width:200px;" class="btn btn-primary">Add</button>
</form>
  <Todo :todo="t" @delete-task="deltask" @update-task="updtask"/>
  <button type="button" class="task" @click="alpha=true">Add Task</button>
  <button type="button" class="clear" @click="t=[]">Clear All</button>
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
</style>
