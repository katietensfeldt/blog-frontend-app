<template>
  <div class="posts-index">
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <router-link :to="`/posts/${post.id}`">
        <img :src="post.image" alt="Post image" />
        <br />
      </router-link>
    </div>
  </div>
</template>

<style>
img {
  width: 200px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      this.posts = response.data;
      console.log(response.data);
    });
  },
};
</script>
