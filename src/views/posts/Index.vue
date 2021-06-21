<template>
  <div class="posts-index">
    <input type="text" list="titles" v-model="searchTerm" placeholder="Search" />
    <br />
    <button v-on:click="sortList('title')">
      Sort by title
      <span v-if="sortAttribute === 'title' && sortOrder === 1">^</span>
      <span v-if="sortAttribute === 'title' && sortOrder === -1">v</span>
    </button>
    <button v-on:click="sortList('created_at')">
      Sort by published date
      <span v-if="sortAttribute === 'created_at' && sortOrder === 1">^</span>
      <span v-if="sortAttribute === 'created_at' && sortOrder === -1">v</span>
    </button>
    <datalist id="titles">
      <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
    </datalist>

    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div
        v-for="post in orderBy(filterBy(posts, searchTerm), sortAttribute, sortOrder)"
        v-bind:key="post.id"
        class="col"
      >
        <div class="card">
          <router-link :to="`/posts/${post.id}`">
            <img :src="post.image" alt="Post image" class="card-img-top" />
          </router-link>
          <div class="card-body">
            <h5 class="card-title">{{ post.title }}</h5>
            <p class="card-text">Published {{ relativeDate(post.created_at) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

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
      sortAttribute: "title",
      sortOrder: 1,
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
    sortList: function (type) {
      if (this.sortAttribute === type) {
        this.sortOrder = this.sortOrder * -1;
      } else {
        this.sortOrder = 1;
        this.sortAttribute = type;
      }
    },
    clicked: function () {
      return true;
    },
  },
};
</script>
