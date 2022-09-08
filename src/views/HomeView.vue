<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Bienvenue sur mon site"/>
    <h2 class="mt-4"><b>My todoApp with vue</b></h2>

    <div class="input-value d-flex m-4">
      <input v-model="task" type="text" placeholder="Entrer une tâche" class="form-control">
      <button @click="submitTask" class="btn btn-success">Ajouter</button>
    </div>

    <div class="task-template m-4">
      <table class="table table-bordered">

      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col"></th>
          <th scope="col"></th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'Finished'}">{{ task.name }}</span>
          </td>
          <td>
            <span @click="changeStatus(index)" 
            :class="{'text-danger': task.status === 'To-do', 
            'text-warning': task.status === 'In-progress', 
            'text-success': task.status === 'Finished'}" 
            class="status">{{ task.status }}</span>
          </td>
          <td class="bg-warning">
           <div class="icons" @click="editTask(index)">
            <span class="fa fa-pen"></span>
           </div>
          </td>
          <td class="bg-danger">
           <div class="icons" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
           </div>
          </td>
        </tr>
      </tbody>  

    </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  },
  
  data() {
    return {
      task:'',
      editedTask: null,
      availableStatuses: ['To-do', 'In-progress', 'Finished'],
      tasks : []
    }
  },
  
  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'To-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
  }

};
</script>

<style lang="scss">
  h2 {
  color: #42b983;
  }
  .input-value {
    margin-top: 2rem;
  }
  .input-value > input {
   padding: 0.5rem;
   border: 2px solid black;
   border-radius: 0.5rem;
   font-size: 1.1rem;
  }
  .btn {
    margin-left: 1rem;
    padding: .6rem;
    border-radius: 0.5rem;
    font-size: 1rem;
  }
  .task-template {
    display: block;
  }
  .icons:hover {
    cursor: pointer;
  }
  .status {
    cursor: pointer;
  }
  .finished {
    text-decoration: line-through;
  }
  .tbody, td, .icons > .fa {
    vertical-align: middle;
  }

</style>
