<template>
  <div class="posts-index">
    <!-- <input type="text" v-model="searchTerm" placeholder="Search" /> -->
    <div v-for="post in orderBy(posts, 'created_at', -1)" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <router-link :to="`/posts/${post.id}`">
        <img :src="post.image" alt="Post image" />
        <br />
      </router-link>
      <p>Published {{ relativeDate(post.created_at) }}</p>
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
import moment from "moment";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      posts: [],
      searchTerm: "",
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      this.posts = response.data;
      console.log(response.data);
    });
  },
  methods: {
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    filterList: function () {
      this.filterBy(this.posts, this.searchTerm, "title");
    },
  },
};
</script>
