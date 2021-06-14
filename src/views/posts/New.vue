<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>New Post</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
        <img v-if="status === 422" src="https://i.ytimg.com/vi/OkA-ZEDt6YQ/maxresdefault.jpg" alt="Status image" />
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="newPostParams.title" />
      </div>
      <div class="form-group">
        <label>Body:</label>
        <input
          v-if="newPostParams.body.length > 45"
          type="text"
          class="input-danger form-control"
          v-model="newPostParams.body"
        />
        <input v-else type="text" class="form-control" v-model="newPostParams.body" />
        <br />
        <small v-if="newPostParams.body.length > 45" class="text-danger">
          {{ newPostParams.body.length }}/45 characters
        </small>
        <small v-else>{{ newPostParams.body.length }}/45 characters</small>
      </div>
      <div class="form-group">
        <label>Image Url:</label>
        <input type="text" class="form-control" v-model="newPostParams.image" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<style scoped>
.text-danger {
  color: red;
}
.input-danger {
  border: 1px solid red;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: { body: "" },
      errors: [],
      status: "",
    };
  },
  methods: {
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "Successfully added new post.";
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          this.status = error.response.status;

          // if (this.status === 404) {
          //   this.$router.push("/404");
          // }
        });
    },
  },
};
</script>
