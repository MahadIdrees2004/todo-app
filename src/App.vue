<script setup>
import { ref , computed, watch } from 'vue'
import Head from './components/head.vue'
import Todo from './components/todo.vue'
import Foot from './components/foot.vue'
import AddTudo from './components/addtudo.vue'
// const t=ref([
//   { id: 1, title: 'Learn Vue.js',description: "I go to learn vue on website", completed: false },
//   { id: 2, title: 'Build a Todo App',description: "I go to build my todo app", completed: false },
//   { id: 3, title: 'Deploy the App',description: "I go to deploy app", completed: false }
// ])

const t = ref([])
const searchQuery = ref('');
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

// Computed filtered todos
const filteredTodos = computed(() => {
  if (!searchQuery.value.trim()) return t.value;
  const q = searchQuery.value.trim().toLowerCase();
  return t.value.filter(
    todo =>
      todo.title.toLowerCase().includes(q) ||
      todo.description.toLowerCase().includes(q)
  );
});
const updstatus = (item) => {
  item.completed = !item.completed;
  localStorage.setItem('tasks', JSON.stringify(t.value))
}
const clearAll = () => {
  t.value = [];
  localStorage.removeItem('tasks');
}

const updindex = ref(-1)
const a = ref(false)
const temp = ref(null);
const add = (temp1) => {
  alert("Task Added Successfully");
  temp.value = temp1;
  t.value.push({
    id: t.value.length + 1,
    title: temp.value.title,
    description: temp.value.description,
    completed: temp.value.completed
  });
  localStorage.setItem('tasks', JSON.stringify(t.value));
  temp.value = { title: '', description: '', completed: false };
  // Navigate back to home after adding
  window.location.hash = '#/';
};
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
// Routing logic
const routes = {
  '/': 'home',
  '/add': 'add',
};
function getRouteFromHash() {
  // Always return a string starting with '/'
  const hash = window.location.hash;
  if (!hash || hash === '#' || hash === '') return '/';
  // Remove leading '#' and ensure leading '/'
  let path = hash.slice(1);
  if (!path.startsWith('/')) path = '/' + path;
  return path;
}
const currentPath = ref(getRouteFromHash());

window.addEventListener('hashchange', () => {
  currentPath.value = getRouteFromHash();
});

const currentView = computed(() => {
  return routes[currentPath.value] || 'notfound';
});

// Hide update form when navigating away from home
watch(currentView, (newView) => {
  if (newView !== 'home') {
    a.value = false;
  }
});
</script>
<template>
  <div class="app-wrapper">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <a class="navbar-brand" href="#/">My ToDo List</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item" :class="{ active: currentView === 'home' }">
            <a class="nav-link" href="#/">Home <span class="sr-only" v-if="currentView === 'home'">(current)</span></a>
          </li>
          <li class="nav-item" :class="{ active: currentView === 'add' }">
            <a class="nav-link" href="#/add">Add Todo</a>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0" @submit.prevent>
          <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" v-model="searchQuery">
        </form>
      </div>
    </nav>

    <!-- Update Task Form (modal style) -->
    <form v-if="a && currentView === 'home'" class="form" @submit.prevent="updaterecord()">
      <h1 class="centr">Update Task</h1>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="title" class="form-control" v-model="temp.title" id="title" placeholder="Enter title" required />
      </div>
      <div class="form-group">
        <label for="desc">Description</label>
        <input type="desc" class="form-control" v-model="temp.description" id="desc" placeholder="Enter Description" required />
      </div>
      <div class="form-group">
        <label for="status">Status</label>
        <select class="form-control" v-model="temp.completed" id="status" required>
          <option :value="false">Pending</option>
          <option :value="true">Completed</option>
        </select>
      </div>
      <button type="submit" style="margin:20px ;width:200px; " class="btn btn-primary centr">Update</button>
    </form>

    <!-- Routing views -->
    <div v-else class="main-content">
      <div v-if="currentView === 'home'">
        <Todo :todo="filteredTodos" @delete-task="deltask" @update-task="updtask" @update-status="updstatus" />
        <button type="button" class="clear" @click="clearAll">Clear All</button>
      </div>
      <div v-else-if="currentView === 'add'">
        <AddTudo @add-task="add" />
      </div>
      <div v-else>
        <h2 class="centr">Page Not Found</h2>
      </div>
    </div>
    <Foot class="footer" />
  </div>
</template>

<style scoped>
html, body, #app {
  height: 100%;
  margin: 0;
  padding: 0;
}
.app-wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f8f9fa;
}
.main-content {
  flex: 1 0 auto;
}
.footer {
  margin-top: auto;
}
.Head {
  background-color: #343a40;
  color: #ffffff;
  padding: 20px;    
  text-align: center;
}
.task {
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
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
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
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}
.centr {
  text-align: center;
  margin: 20px auto;
}
.nav{
  background-color: #343a40;
  color: #ffffff;
  padding: 10px;
}
.navbar .form-inline {
  display: flex !important;
  align-items: center !important;
  flex-wrap: nowrap !important;
  margin-bottom: 0 !important;
}
.navbar .form-inline .form-control {
  margin-right: 8px !important;
  margin-bottom: 0 !important;
}
.navbar .form-inline .btn {
  margin-top: 0 !important;
  margin-bottom: 0 !important;
}
</style>
