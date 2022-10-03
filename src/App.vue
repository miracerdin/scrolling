<template>
  <div id="app">
    <InfiniteScroll :articles="articles" @refetch="fetch">
      <!-- <template v-slot:item="{ item }">
        <div>{{ item.name }}</div>
      </template> -->
    </InfiniteScroll>
  </div>
</template>
<script lang="ts">
import { Component } from "vue-property-decorator";
import Vue from "vue";
import InfiniteScroll from "./components/InfiniteScroll.vue";
import axios from "axios";
@Component({
  components: {
    InfiniteScroll,
  },
})
export default class App extends Vue {
  articles: string[] = [];
  offset = 100;
  lastpokemon = 100;

  async fetch(offset: number) {
    if (this.lastpokemon >= 1200) {
      return;
    }
    let articles = await axios
      .get(`https://pokeapi.co/api/v2/pokemon/?offset=${offset}&limit=100`)
      .then((response) => {
        // console.log("response", response.data.results);
        for (let i of response.data.results) {
          let datas = axios.get(i.url).then((response) => {
            console.log("response", response.data);
            this.articles.push(response.data);
          });
        }
      })
      .catch((error) => console.log(error));
    // console.log(articles.data.results);
    // console.log("articles", articles);
    // this.articles.push(...articles.data.res);
    this.lastpokemon += 100;
    console.log("articles", this.articles);
  }
  mounted() {
    this.fetch(0);
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
