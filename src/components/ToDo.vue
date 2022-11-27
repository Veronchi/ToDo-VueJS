<template>
  <div class="container">
    <h2 class="text-center mt-5">ToDo List</h2>
    <div class="d-flex mb-5">
      <input v-model="newTask" type="text" placeholder="Enter your task" class="form-control me-1">
      <button @click="addTask" class="btn btn-warning rounded-1">Add</button>
    </div>
    <table class="table">
      <thead class="container">
        <tr class="row">
          <th class="col-8" scope="col">Task</th>
          <th class="col-2" scope="col">Status</th>
          <th class="col-1" scope="col">#</th>
          <th class="col-1" scope="col">#</th>
        </tr>
      </thead>
      <tbody class="container">
        <tr class="row" v-for="(task, idx) in tasks" :key="idx">
          <td class="col-8 text-start">
            <span v-if="!task.edit" :class="{ 'finished': task.statusId === 2 }">
              {{ firstCharUpper(task.name) }}
            </span>
            <input v-show="task.edit" v-model="task.name" type="text" placeholder="Enter your changes"
              class="edit-input">
          </td>
          <td class="col-2">
            <select class="form-select" id="inputGroupSelect01" v-model="task.statusId">
              <option v-for="(status, idx) in validStatuses" :key="idx" :value="status.id">
                {{ status.name }}
              </option>
            </select>
          </td>
          <td class="col-1">
            <div class="text-center" @click="editTask(idx)" role="button">
              <font-awesome-icon v-if="!task.edit" icon="fa-solid fa-pen" />
              <font-awesome-icon v-else icon="fa-solid fa-check" />
            </div>
          </td>
          <td class="col-1">
            <div class="text-center" @click="removeTask(idx)" role="button">
              <font-awesome-icon icon="fa-solid fa-trash" />
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>



export default {
  name: 'ToDo',
  props: {
    msg: String
  },
  data() {
    return {
      newTask: '',
      validStatuses: [
        { id: 0, name: "to do" },
        { id: 1, name: "in progress" },
        { id: 2, name: "finished" }
      ],
      tasks: [
        {
          name: 'Buy some milk',
          edit: false,
          statusId: 0
        },
        {
          name: 'Drink coffee',
          edit: false,
          statusId: 1
        }
      ]
    }
  },
  methods: {
    addTask() {
      if (this.newTask.length !== 0) {
        this.tasks.map(i => i.edit = false)
        this.tasks.push({
          name: this.newTask,
          statusId: 0,
          edit: false
        });

        this.newTask = "";
      }
    },
    removeTask(idx) {
      this.tasks.splice(idx, 1);
    },
    editTask(idx) {
      this.tasks[idx].edit = !this.tasks[idx].edit;
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
}
</script>

<style scoped>
.finished {
  text-decoration: line-through;
}

.edit-input {
  width: 100%;
}
</style>
