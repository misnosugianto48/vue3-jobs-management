<!-- !Composition API -->

<script setup>
import { onMounted, ref } from 'vue';

const name = ref('Misno Sugianto');
const status = ref('active');
const tasks = ref(['Task One', 'Task Two', 'Task Three', 'Task Four']);
const newTask = ref('');

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  } else if (status.value === 'pending') {
    status.value = '';
  } else {
    status.value = 'active';
  }
}

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log('Error fetching tasks');
  }
});
</script>

<template>
  <h1>Hello {{ name }}</h1>
  <br>

  <p v-if="status === 'active'">Misno is Aktif</p>
  <p v-else-if="status === 'pending'">Misno is Pending</p>
  <p v-else> Misno is Inactive</p>
  <br>

  <form action="" @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <br>
    <input type="text" name="newTask" id="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>
  <br>

  <h3>Tasks:</h3>
  <ul>
    <!--! looping for in-->
    <li v-for="(task, index) in tasks" :key="task">{{ task }}
      <button @click="deleteTask(index)">x</button>
    </li>

  </ul>
  <br>

  <!--! <a v-bind:href="link">Click on link</a> -->
  <div>
    <a :href="link">Click on link</a>
  </div>

  <div>
    <!--! <button v-on:click="toggleStatus">Change Status Misno</button> -->
    <button @click="toggleStatus">Change Status Misno</button>
  </div>
</template>


<style>
a {
  cursor: pointer;
}

button {
  cursor: pointer;
  border: none;
  padding: 12px;
  background-color: aqua;
  font-weight: bold;
  border-radius: 5px;
}

input {
  padding: 10px;
  margin-right: 10px;
}
</style>
