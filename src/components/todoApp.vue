<template>
  <div class="container">
    <h2 class="text-center mt-5">My ToDo App</h2>

    <div class="d-flex mt-5">
      <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
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
      <tr v-for="(task,index) in tasks" :key="index">
        <th>
          <span :class="{'finished' : task.status === 'finished' }">
            {{ task.name }}
          </span>
        </th>
        <td style="width: 120px">
          <span @click="changeStatus(index)" class="pointer" :class="{'text-danger' : task.status === 'to-do',
          'text-warning' : task.status === 'in-progress',
           'text-success' : task.status === 'finished'}">
            {{ firstCharUpper(task.status) }}
          </span>
        </td>
        <td>
          <div @click="editTask(index)"  class="text-center">
            <span class="fa fa-pen-alt"></span>
          </div>
        </td>
        <td>
          <div @click="deleteTask(index)" class="text-center">
            <span class="fa fa-trash-alt"></span>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data(){
    return{
      task : "" ,
      editedTask : null,
      availableStatuses : ["to-do","in-progress","finished"],
      tasks : [
        {
          name : "Do Your Homework",
          status : "To-do",
        },
        {
          name : "Cook Meal",
          status : "In-rogress",
        }
      ]
    }
  },
  methods:{
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        })
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = ""
    },
    deleteTask(index){
      this.tasks.splice(index,1);
    },
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask=index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<style scoped>
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>
