<template>
  <div id="app">
    <InfiniteScroll></InfiniteScroll>
    <!-- <div>{{ articles }}</div> -->
    <div v-for="article in articles" :key="article.name">
      <div>{{ article.name }}</div>
      <!-- <h1>{{ article.name }}</h1>
      <p>{{ article.email }}</p>
      <p>{{ article.body }}</p> -->
    </div>
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>|
      <router-link to="/infinitescroll">InfiniteScroll</router-link>
    </nav>
    <router-view /> -->
    <div
      v-if="articles.length"
      v-observe-visibility="handleScrollToBottom"
    ></div>
  </div>
</template>
<script lang="ts">
import { Component } from "vue-property-decorator";
import Vue from "vue";
import axios from "axios";
import InfiniteScroll from "./components/InfiniteScroll.vue";
@Component({
  components: {
    InfiniteScroll,
  },
})
export default class App extends Vue {
  articles: string[] = [];
  limit = 100;
  offset = 0;
  async fetch() {
    let articles = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/?offset=${this.offset}&limit=${this.limit}`
    );
    console.log(articles.data.results);
    this.articles.push(...articles.data.results);
  }
  handleScrollToBottom(isVisible: boolean) {
    if (!isVisible) {
      return;
    }
    if (this.offset >= 1200) {
      return;
    }
    // this.limit += 10;
    this.offset += 100;
    this.fetch();
  }
  mounted() {
    this.fetch();
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
