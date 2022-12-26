<template>
  <div class="container">
    <h2 class="text-center mt-5">My To Do App</h2>

    <!--input-->
    <div class="d-flex">
      <input
        type="text"
        name="addTask"
        id="addTask"
        placeholder="Enter Task"
        class="form-control"
        v-model="taskName"
      />
      <button type="submit" @click="addTask" class="btn btn-warning rounded-0">
        Add
      </button>
    </div>

    <!-- Task List -->
    <h3 v-if="tasks.length === 0" class="mt-5">
      Please add task to create list.
    </h3>
    <table v-else class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col" style="width: 5%"></th>
          <th scope="col">Task Name</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(task, index) in tasks"
          :key="`${index}*${Math.floor(Math.random()) * 10000}`"
        >
          <td>
            <div class="text-center">
              <input
                type="checkbox"
                @click="changeStatus(index)"
                :id="`${task.name}_${index}_checkbox`"
                :checked="task.done"
              />
            </div>
          </td>
          <td>{{ task.name }}</td>
          <td>{{ task.done ? "Done" : "Pending" }}</td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash" style="cursor: pointer"> </span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <h3 class="text-center mt-5">
      completed items:{{ completedTasks.length > 0 ? completedTasks : 0 }}
    </h3>
  </div>
</template>

<script>
function sortingByStatus(tasks = []) {
  tasks.sort((a, b) => {
    return Number(b.done) - Number(a.done);
  });
  return tasks;
}
function getSortedTasks(tasks = []) {
  tasks.sort(function (a, b) {
    const nameA = a.name.toUpperCase();
    const nameB = b.name.toUpperCase();

    if (nameA < nameB) {
      return -1;
    }
    if (nameA > nameB) {
      return 1;
    }

    return 0;
  });

  const doneStatusTasks = tasks.filter((task) => {
    return task.done === true;
  });

  if (doneStatusTasks.length >= 1) {
    tasks = sortingByStatus([...tasks]);
  }

  return tasks;
}

export default {
  name: "ToDo",
  data() {
    return {
      taskName: "",
      tasks: [],
      completedTasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.taskName.replace(/^\s+|\s+$/gm, "").length) {
        console.log(`${this.taskName} is added to to-do list`);
        var duplicateTask = this.tasks.filter((task) => {
          return (
            task.name.toLowerCase() ===
            this.taskName.toLowerCase().replace(/^\s+|\s+$/gm, "")
          );
        });

        if (duplicateTask.length) {
          console.log("Already present");
          return;
        }
        const taskObj = {
          name: this.taskName,
          done: false,
        };
        this.tasks.push(taskObj);
      } else {
        console.log(`Please Enter the task`);
        return;
      }
      const sortedTasks = getSortedTasks([...this.tasks]);
      this.tasks = [...sortedTasks];
      console.log(`sorted tasks are`, sortedTasks);

      this.taskName = "";
    },
    deleteTask(index) {
      console.log(`${this.tasks[index].name} is deleted successfully`);
      this.tasks.splice(index, 1);
    },
    changeStatus(index) {
      console.log(`${this.tasks[index].name} is status changed successfully`);
      this.tasks[index].done = !this.tasks[index].done;

      console.log(this.completedTasks);
      setTimeout(() => {
        console.log(this.tasks);
        this.tasks.sort((a, b) => {
          return Number(b.done) - Number(a.done);
        });
      }, 0);
      const completedTasksObj = [...this.tasks].filter((task) => {
        return task.done === true;
      });
      for (let i = 0; i < completedTasksObj.length; i++) {
        if (this.completedTasks.indexOf(completedTasksObj[i].name) === -1) {
          this.completedTasks.push(completedTasksObj[i].name);
        }
      }
      //const sortedTasks = sortingByStatus([...this.tasks]);
      // this.tasks = [];
      //this.tasks = [...sortedTasks];
    },
  },
};
</script>


