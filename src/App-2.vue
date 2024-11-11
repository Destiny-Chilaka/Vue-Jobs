<script setup>
import {onMounted, ref} from 'vue';

  // data(){
  //   return{
  //     name:"john doe",
  //     status:'pending',
  //     tasks:["task one","task two","task three"],
  //     link:"https://google.com",
  //   };
  // },
  // methods:{
  //   toggleStatus(){
  //     if (this.status === 'active'){
  //       this.status = 'pending';
  //     } else if(this.status === 'pending'){
  //       this.status = 'inactive';
  //     }else{
  //       this.status = 'active';
  //     }
  //   },
  // },
 
    const name =ref("John Doe");
    const status = ref("active");
    const tasks = ref( ["Task One", "Task Two", "Task Three"]);
    const newTask =ref('');

    const toggleStatus = () =>{
      if (status.value === 'active'){
         status.value = 'pending';
       } else if(status.value === 'pending'){
         status.value = 'inactive';
       }else{
         status.value = 'active';
       }
     };

     const addTask = () =>{
      if(newTask.value.trim() !== ''){
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
     };

     const deleteTask = (index) =>{
      tasks.value.splice(index,1);
     };

     onMounted(async()=>{
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/todos');
        const data = await response.json();
        tasks.value = data.map((task) => task.title )
      }catch(error){
      console.log('Error fetching tasks')
      }
     });

     
   
</script>


<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>


<form @submit.prevent="addTask">
  <label for="newTask"> Add Task</label>
  <input type="text" name="newTask" id="newTask" v-model="newTask">
  <button type="submit"> Submit</button>
</form>
  <h2>
    Tasks:
  </h2>
  <ul >
<li v-for="(n, index) in tasks" :key="n">
  <span>{{ n }}</span>
  <button @click="deleteTask(index)">x</button>
</li>
  </ul>

  <br>
  <button v-on:click="toggleStatus"> Change Status</button>
</template>

