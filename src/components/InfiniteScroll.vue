<template>
  <div class="container">
    <div class="eachOne" v-for="item in articles" :key="item.name">
      <slot name="item" v-bind:item="item"></slot>
      <h3>{{ item.name }}</h3>
      <div>
        <img
          :src="item.sprites.other.dream_world.front_default"
          alt="pokemonImage"
        />
      </div>

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
import { Component, Prop, Vue } from "vue-property-decorator";
@Component
export default class InfiniteScroll extends Vue {
  @Prop({ required: true }) articles!: [
    { name: string; url: string; sprites: { front_default: string } }
  ];
  // datas: object[] = [];
  offset = 0;
  // limit = 100;
  async handleScrollToBottom(isVisible: boolean) {
    if (!isVisible) {
      return;
    }
    // if (this.offset >= 1200) {
    //   return;
    // }
    // this.limit += 10;
    this.offset += 100;
    this.$emit("refetch", this.offset);
  }
  // mounted() {
  //   this.datas = this.articles;
  // }
}
</script>

<style>
.container {
  display: flex;
  flex-wrap: wrap;
}

.eachOne {
  margin: 1rem auto;
  background: #ccc;
  width: 200px;
  border-radius: 1em;
}
img {
  width: 80%;
  height: 100px;
  object-fit: contain;
}
</style>
