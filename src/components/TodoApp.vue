<template>
  <div class="container">
    <h2 class="text-center mt-5">TO-DO APP</h2>

    <div class="d-flex">
      <input v-model="newTask" type="text" placeholder="Enter Your Task" class="form-control">
      <button @click="submitTask" class="btn btn-dark text-white rounded-0">Submit</button>
    </div>

    <table v-if="tasks.length > 0" class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished : task.status === 'finished'}">
              {{ task.name }}
            </span>
          </td>
          <td style="width: 250px;">
            <span @click="changeStatus(index)" class="point"
              :class="{
                'text-danger' :  task.status === 'to-do',
                'text-warning' : task.status === 'in-progress',
                'text-success' : task.status === 'finished'
              }">
              {{ capitalizeFirst(task.status) }}
            </span>
          </td>
          <td>
            <div class="text-center point" @click="editTask(index)">
              <span class="fa fa-pen text-primary"></span>
            </div>
          </td>
          <td>
            <div class="text-center point"  @click="deleteTask(index)">
              <span class="fa fa-trash text-danger"></span>
            </div>
          </td> 
        </tr>
      </tbody>
    </table>
    <p v-else class="text-center mt-3">No tasks available</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const tasks = ref([]);

const newTask = ref('');
const editingIndex = ref(-1);
const statuses = ["to-do", "in-progress", "finished"];

// Function to add or update a task
const submitTask = () => {
  if (!newTask.value.trim()) {
    return; 
  }

  if (editingIndex.value === -1) {
    tasks.value.push({
      name: newTask.value,
      status: "to-do" 
    });
  } else {
    tasks.value[editingIndex.value].name = newTask.value;  
    editingIndex.value = -1;
  }

  newTask.value = '';
};

// Function to delete a task by index
const deleteTask = (index) => {
  tasks.value.splice(index, 1); 
};

// Function to edit a task by index
const editTask = (index) => {
 
  newTask.value = tasks.value[index].name;
  editingIndex.value = index; 
};

const changeStatus = (index) => {
  const currentIndex = statuses.indexOf(tasks.value[index].status);
  const nextIndex = (currentIndex + 1) % statuses.length; 
  tasks.value[index].status = statuses[nextIndex];
};

const capitalizeFirst = (str) => {
  return str.charAt(0).toUpperCase() + str.slice(1);
};
</script>

<style scoped>
.point {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
  color: green;
}
</style>
