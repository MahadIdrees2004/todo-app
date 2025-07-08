
<template class ="Addtudo">
  
      <form class="form" @submit.prevent="add">
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
  <button type="submit" style="margin:20px auto;width:200px;" class="btn btn-primary centr">Add</button>
</form>
</template>
<script setup>
const temp = ref({
    title: '',
    description: '',
    completed: false
});
import { ref } from 'vue';
const emit = defineEmits(['add-task']);
const add = () => {
    emit('add-task', temp.value);
    // Optionally reset the form after adding
    temp.value = { title: '', description: '', completed: false };
};
</script>