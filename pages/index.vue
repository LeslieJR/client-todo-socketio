<template>
  <v-app>
    <div class="index-page">
      <div class="container">
        <div class="form-container">
          <TForm :taskCreated="taskCreated" />
        </div>
        <div class="list-container">
          <TTask
            class="mb-4"
            v-for="task in tasks"
            :key="task.id"
            :title="task.title"
            :description="task.description"
            :photo="task.photo"
          />
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
export default {
  head: {
    script: [
      {
        src: "http://localhost:3500/socket.io/socket.io.js",
      },
    ],
  },
  data() {
    return {
      tasks: [],
      socket: undefined,
    };
  },
  async beforeMount() {
    await this.updateTasks();
  },
  mounted() {
    //para poder acceder a 'http://localhost:3500/socket.io/socket.io.js'
    this.socket = io("http://localhost:3500");

    this.socket.on("task:update", async () => {
      await this.updateTasks();
    });
  },
  methods: {
    taskCreated(task) {
      this.socket.emit("task:created", { taskId: task.id });
    },
    async updateTasks() {
      const res = await fetch("http://localhost:3500/api/task/all");
      const data = await res.json();

      this.tasks = data.tasks;
    },
  },
};
</script>
<style scoped>
.container {
  display: grid;
  column-gap: 10px;
  grid-template-columns: 40% 60%;
}
</style>