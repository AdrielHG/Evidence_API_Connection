<script>
import axios from "axios";

export default {
  data() {
    return {
      id: "",
      note: {}
    };
  },
  methods: {
    fetchNote() {
      if (this.id) {
        axios.get(`http://127.0.0.1:8000/api/note/${this.id}`)
          .then(response => {
            this.note = response.data;
          })
          .catch(error => {
            console.error("Error fetching note:", error);
            this.note = { error: "Note not found. Please check the ID and try again." };
          });
      }
    }
  }
}

</script>

<template>

<div class="container mt-5">
    <h2 class="text-center">Note Content by ID</h2>
    <div class="row justify-content-center">
      <div class="col-md-6">
        <form id="searchForm" class="form-inline" @submit.prevent="fetchNote">
          <div class="mb-3">
            <label for="idInput" class="form-label">Note ID:</label>
            <input type="text" v-model="id" class="form-control" id="idInput" placeholder="Enter ID" required>
          </div>
          <button type="button" class="btn btn-primary" @click="fetchNote">Search</button>
        </form>
      </div>
    </div>
    <div class="row justify-content-center mt-4" v-if="note">
      <div class="col-md-6">
        <div id="result" class="alert alert-info">
          <p v-if="note.id">Note ID: {{ note.id }}</p>
          <p v-if="note.title">Title: {{ note.title }}</p>
          <p v-if="note.content">Content: {{ note.content }}</p>
          <p v-if="note.error" class="text-danger">{{ note.error }}</p>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>

</style>
