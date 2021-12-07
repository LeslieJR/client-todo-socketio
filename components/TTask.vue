<template>
  
    <v-card>
      <div class="d-flex flex-no-wrap">
        <v-avatar class="ma-3" size="125" tile>
          <v-img :src="photo"></v-img>
        </v-avatar>
        <div>
          <v-card-title>{{ title }}</v-card-title>
          <v-card-subtitle>{{ description }}</v-card-subtitle>
        </div>
      </div>
    </v-card>
  
</template>
<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    photo: {
      type: String,
      required: true,
    },
  },
};
</script>
<style scoped>

</style>