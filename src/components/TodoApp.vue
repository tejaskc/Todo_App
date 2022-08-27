<template>
  <div class="container" style="max-width: 600px">
    <h2 class="text-center mt-5">Vue Todo List Application</h2>
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter todo task"
        class="w-100 form-control"
      />
    </div>
    <div class="d-flex mt-2">
      <button class="btn btn-success" @click="submitTask">
        SUBMIT
      </button>
    </div>
    <table class="table table-hover mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
              <button class="btn btn-danger btn-sm" @click="deleteTask(index)">
                Delete
              </button>
          </td>
          <td class="text-center">
            <button class="btn btn-info btn-sm" @click="editTask(index)">
                Edit
              </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Shopping in D-Mart",
          status: "to-do",
        },
        {
          name: "Read a Vue JS book",
          status: "in-progress",
        },
        {
          name: "Watch IPL Match",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    submitTask() {
      if (this.task.length === 0) return;

      /* update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.line-through {
  text-decoration: line-through;
}
</style>
