<template>
  <v-card>
    <v-img :src="url" class="width-100"></v-img>
    <v-list-item>
      <input id="file" ref="file" type="file" @change="onChange" />
    </v-list-item>
    <v-list-item>
      <v-text-field
        solo
        dense
        clearable
        hide-details="auto"
        placeholder="title"
        v-model="title"
      />
    </v-list-item>
    <v-list-item>
      <v-text-field
        solo
        dense
        clearable
        hide-details="auto"
        placeholder="description"
        v-model="description"
      />
    </v-list-item>
    <v-card-actions class="d-flex justify-center">
      <v-btn class="white--text" color="indigo" @click="onClick">Submit</v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  props: {
    taskCreated: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      url: "",
      title: "",
      description: "",
    };
  },
  methods: {
    onChange() {
      //document.getElementeById("file") <=> this.$refs.file

      const file = this.$refs.file.files[0];

      //para codificar el archivo
      //https://developer.mozilla.org/en-US/docs/Web/API/FileReader
      const reader = new FileReader();
      //si hay un archivo
      if (file) {
        reader.readAsDataURL(file);
      }
      reader.onloadend = () => {
        this.url = reader.result;
      };
    },
    async onClick() {
      try {
        const body = JSON.stringify({
          title: this.title,
          description: this.description,
          photo: this.url,
        });

        const res = await fetch("http://localhost:3500/api/task/create", {
          method: "post",
          headers: {
            "Content-Type": "application/json",
          },
          body,
        });
        const data = await res.json();

        // BEGIN SOCKET
        this.taskCreated(data.task);
        // END SOCKET
        (this.title = ""), (this.description = ""), (this.url = "");
      } catch (e) {
        console.log("error: ", e.message);
      }
    },
  },
};
</script>
<style scoped>
.t-form {
  display: grid;
  grid-template-columns: 1fr;
  row-gap: 5px;
  background: #feca57;
  padding: 10px;
}
</style>

