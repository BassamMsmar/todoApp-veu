<template>
  <div class="container">
    <h2>Todo App</h2>
    <div class="d-flex mt-5">
      <input type="text" placeholder="Enter Task" class="form-control" v-model="task" />
      <button type="button" class="btn btn-warning ms-3" @click="submitTask">Add</button>
    </div>
    <div class="mt-5">
      <table class="table table-hover">
        <thead class="table-light">
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col">#</th>
            <th scope="col">#</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <th scope="row">{{task.name}}</th>
            <td>{{task.status}}</td>
            <td>
              <div @click="editTask(index)">
                <samp><i class="fa-regular fa-pen-to-square"></i></samp>
              </div>
            </td>
            <td>
              <div @click="deleteTask(index)"> 
                <samp><i class="fa-regular fa-trash-can"></i></samp>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-app',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      tasks : [
        {
          name: 'Stady js',
          status: 'todo',
        },
        {
          name: 'Stady python',
          status: 'doing',
        },
        {
          name: 'Stady c++',
          status: 'inprogress',
        }
      ]
    }
  },

  methods: {
    submitTask() {
      if (this.task == '') return
      this.tasks.push({name: this.task, status: 'todo'})
      this.task = ''
      
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },

    editTask(index) {
      this.task = this.tasks[index].name
    }

  }


}
</script>
