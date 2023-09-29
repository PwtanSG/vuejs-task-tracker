<template>
  <div class="container">
    <Header
      :showAddTask="showAddTask"
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
    />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    Header,
    Footer,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },

    async addTask(task) {
      console.log(task);
      // this.tasks = [...this.tasks, task];
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },

    async deleteTask(id) {
      // console.log("app level", id);
      if (confirm("confirm delete?")) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        // update tasks data
        // res.status === 200
        //   ? this.tasks = this.tasks.filter(task=>task.id = id)
        //   : alert('Error deleting task')

        res.status === 200
          ? (this.tasks = await this.fetchTasks())
          : alert("Error deleting task");
      }
    },

    async toggleReminder(id) {
      // console.log("toggle reminder");
      const taskToToggle = await this.fetchTask(id);
      // console.log(taskToToggle);
      const updateTaskReminder = {
        ...taskToToggle,
        reminder: !taskToToggle.reminder,
      };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updateTaskReminder),
      });

      //reload updated tasks
      const data = await res.json()
      // console.log(data)
      this.tasks = this.tasks.map((task) =>
        task.id == id ? { ...task, reminder: !data.reminder } : task
      );

      // res.status === 200
      //   ? (this.tasks = await this.fetchTasks())
      //   : alert("Error updating task");
    },

    async fetchTasks() {
      const res = await fetch("api/tasks"); //refer to vue.config.js
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
  
  async created() {
    this.tasks = await this.fetchTasks();
    // this.tasks = [
    //   {
    //     id: 1,
    //     text: "buy Food",
    //     day: "19 March 2023",
    //     reminder: true,
    //   },
    //   {
    //     id: 2,
    //     text: "Pay rent",
    //     day: "22 March 2023",
    //     reminder: false,
    //   },
    //   {
    //     id: 3,
    //     text: "buy ticket",
    //     day: "19 April 2023",
    //     reminder: false,
    //   },
    // ];
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
