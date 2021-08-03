<template>
  <div class="container">
    <h2 class="text-center mt-5">ToDo App</h2>

    <div class="d-flex" v-if="!isEditing">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
        @keyup.enter="submitTask"
      />
      <button id="btn-1" class="btn btn-warning rounded-0" @click="submitTask">
        Submit
      </button>
    </div>
    <div class="d-flex" v-else>
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
        @keyup.enter="submitTask"
      />
      <button id="btn-1" class="btn btn-warning rounded-0" @click="submitTask">
        update
      </button>
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
          <th>
            <span
              class="normal"
              :class="{ finished: task.status === 'finished' }"
            >
              {{ firstCharUpper(task.name) }}
            </span>
          </th>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer normal">
              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td>
            <div class="text-center pointer" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center pointer" @click="deleteTask(index)">
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
  data() {
    return {
      isEditing: false,
      editedTask: null,
      task: "",
      availableStatus: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "slap 10 people",
          status: "to-do",
        },
        {
          name: "run 100m in 7s",
          status: "in-progress",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.isEditing = true;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newindex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newindex > 2) newindex = 0;
      this.tasks[index].status = this.availableStatus[newindex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}

.normal {
  font-weight: 200;
}
#btn-1 {
  transition: 0.7s ease;
}
#btn-1:hover {
  background-color: aqua;
}
</style>
