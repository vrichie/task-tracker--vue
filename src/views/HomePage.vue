<template>
  <AddTask @add-task="addTask" v-show="showAddTask" />

  <TaskFile
    :tasks="tasks"
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
  />
</template>
<script>
import TaskFile from "../components/TaskFile";
import AddTask from "../components/AddTask";

export default {
  name: "HomePage",
  props: {
    showAddTask: Boolean,
  },
  components: {
    TaskFile,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("http://localhost:5000/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    // addTask(task) {
    //   this.tasks = [...this.tasks, task];
    // },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    // toggleReminder(id) {
    //   this.tasks = this.tasks.map((task) =>
    //     task.id === id ? { ...task, reminder: !task.reminder } : task
    //   );
    // },
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "DELETE",
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("failed to delete");
      }
    },
    // deleteTask(id) {
    //   if (confirm("Are you sure?"))
    //     this.tasks = this.tasks.filter((task) => task.id !== id);
    // },
    async fetchTasks() {
      const res = await fetch("http://localhost:5000/tasks");
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
    //     text: "Doctors Appointment",
    //     day: "March 1st at 2:30pm",
    //     reminder: true,
    //   },
    //   {
    //     id: 2,
    //     text: "Teachers Appointment",
    //     day: "March 2nd at 2:30pm",
    //     reminder: false,
    //   },
    //   {
    //     id: 3,
    //     text: "Students Appointment",
    //     day: "March 3rd at 2:30pm",
    //     reminder: true,
    //   },
    //   {
    //     id: 4,
    //     text: "Girlfriends Appointment",
    //     day: "March 4st at 2:30pm",
    //     reminder: true,
    //   },
    // ];
  },
};
</script>