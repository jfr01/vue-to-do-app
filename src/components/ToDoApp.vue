<template>
  <div class="container">
    <h2 class="text-center mt-5">Vue To Do App</h2>
  
    <div class="d-flex">
      <input v-model="newTask" type="text" placeholder="Enter Task" class="form-control">
      <button @click="submitNewTask" class="btn btn-warning rounded-0">Submit</button>
    </div>
  <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td><span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span></td>
      <td style="width: 120px">
        <span @click="changeStatus(index)" class="pointer"
        :class="{'text-danger': task.status === 'to-do',
        'text-warning': task.status === 'in-progress',
        'text-success': task.status === 'finished'
        }"
        
        >{{ task.status }}</span></td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      newTask: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'Get bananas',
          status: 'to-do'
        },
        {
          name: 'Get chocolates',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitNewTask(){
      // Returns if input field is empty!
      if(this.newTask.length === 0) return;


      // Otherwise, push newTask to tasks array
      if(this.editedTask === null){
      this.tasks.push({
        name: this.newTask,
        status: 'to-do'
      })
      } else {
        this.tasks[this.editedTask].name = this.newTask;
        this.editedTask = null 
      }
      this.newTask = '';
    },

    deleteTask(index){
      // Uses Splice to remove the first index in task array
      this.tasks.splice(index, 1)
    },

    editTask(index){
      this.newTask = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex]
    }
  }
}
</script>

<style scoped>

.pointer {
  cursor: pointer;
}

.finished{
  text-decoration: line-through;
}

</style>
