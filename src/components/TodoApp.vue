<template>
  <div class="container-sm">
    <form @submit.prevent="submitTask">
      <h1 class="text-center mt-3 ">create your Todo list</h1>
      <div class="d-flex  align-items-center">
        <input class="form-control  rounded-0" v-model="task" type="text" placeholder="Enter Task">
        <button class="btn btn-warning rounded-0">Submit</button>
      </div>
    </form>
    </div>
    <div class="container-sm">
      <table class="table table-striped-columns mt-5 text-center border">
        <thead>
          <tr>
            <th scope="col">task</th>
            <th scope="col" style="width:200px">status</th>
            <th scope="col" style="width:120px">Edit</th>
            <th scope="col" style="width:120px">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task , index) in  tasks" :key="index">
            <td :class="{'finished' : task.status === 'finished'}">{{ task.name }}</td>
            <td style="width:200px">
              <span 
                :class="{'text-danger': task.status === 'to-do',
                  'text-warning' : task.status === 'in-progress',
                  'text-success' : task.status === 'finished'
                }"
                class="pointer" @click="changeStatus(index)">
                {{task.status}} 
              </span>
            </td> 
            <td style="width:120px" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </td>
            <td style="width:120px" @click="deleteTask">
              <span class="fa fa-trash"></span>
            </td> 
          </tr>
        </tbody>
      </table>
    </div>
 </template>

<script>
export default{
  data(){
    return{
        task: '',
        tasks: [],
        editedTask: null,
        availableStatuses:['to-do', 'in-progress', 'finished'],
    }
  },

  methods :{
    submitTask() {
      if (this.task.length === 0) return;
      if(this.editedTask === null){
      this.tasks.push({
      name: this.task,
      status: "to-do"
    })
      } else {
                this.tasks[this.editedTask].name = this.task
                this.editedTask = null
        }
          this.task = ''
    },

    deleteTask(index) {
      this.tasks.splice(index, 1)
    },

    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      newIndex++
      if(newIndex > 2) {
        newIndex = 0
      }
      this.tasks[index].status = this.availableStatuses[newIndex]
    },

    togglebutton() {
      this.paraisvisible =! this.paraisvisible
    },
  }
}
</script>

<style>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration:line-through
}
</style>