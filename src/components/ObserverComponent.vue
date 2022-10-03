<template><div class="observer">Observer</div></template>

<script lang="ts">
import Vue from "vue";

export default class Observer extends Vue {
  observer = null;
  mounted() {
    this.observer = new IntersectionObserver(([entry]) => {
      if (entry && entry.isIntersecting) {
        this.emit("intersect");
      }
      this.observer.observe(this.$el);
    });
  }
}
</script>

<style></style>
