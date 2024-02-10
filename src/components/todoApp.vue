<template>
  <body>
    <div class="container">
      <h2 class="text-center pt-4">Todo App</h2>
      <div class="d-flex mt-5">
        <input
          type="text"
          placeholder="Enter Task"
          class="form-control"
          v-model="task"
        />
        <button type="button" class="btn btn-warning ms-3" @click="submitTask">
          Add
        </button>
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
            <tr v-for="task in tasks" :key="task.id">
              <th scope="row">
                <span :class="{ finished: task.status == 'doing' }">
                  {{ task.name }}
                </span>
              </th>
              <td>
                <span
                  @click="editStatus(task.id)"
                  class="poinyrt"
                  :class="{
                    'text-danger': task.status == 'todo',
                    'text-warning': task.status == 'inprogress',
                    'text-success': task.status == 'doing',
                  }"
                >
                  {{ task.status }}
                </span>
              </td>
              <td>
                <div @click="editTask(task.id)">
                  <samp class="poinyrt">
                    <i class="fa-regular fa-pen-to-square"></i
                  ></samp>
                </div>
              </td>
              <td>
                <div @click="deleteTask(task.id)">
                  <samp class="poinyrt"
                    ><i class="fa-regular fa-trash-can"></i
                  ></samp>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </body>
</template>

<script>
import axios from 'axios';

export default {
  name: "todo-app",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      isEditTask: null,
      tasks: [],
    };
  },

  mounted() {
    this.getTasks();
  },

  methods: {
    getTasks(){

        axios({
            method:"get",
            url:"http://127.0.0.1:8000/tasks/api",
          }).then(response => this.tasks = response.data)
        },

    submitTask() {
      if (this.task == "") return;
      if (this.isEditTask == null) {
        this.tasks.push({ name: this.task, status: "todo" });
      } else {
        this.tasks[this.isEditTask].name = this.task;
        this.isEditTask = null;
      }

      this.task = "";
    },
    editStatus(index) {
      if (this.tasks[index].status == "todo") {
        this.tasks[index].status = "inprogress";
      } else if (this.tasks[index].status == "inprogress") {
        this.tasks[index].status = "doing";
      } else if (this.tasks[index].status == "doing") {
        this.tasks[index].status = "todo";
      }
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      (this.task = this.tasks[index].name), (this.isEditTask = index);
    },
  },
};
</script>

<style scoped>
body {
  background-color: rgb(26, 69, 105);
  width: 100%;
  height: 1200px;
}
.poinyrt {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
