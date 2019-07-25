<template>
  <div id="app">
    <form @submit.prevent="searchGifs">
      <label>Search</label>
      <input v-model="search" />
    </form>
    <div class="loading" v-if="loading">
      <img src="https://miro.medium.com/max/882/1*9EBHIOzhE1XfMYoKz1JcsQ.gif" alt="loading" />
    </div>
    <div class="images" v-if="!loading">
      <img v-for="gif in gifs" :src="gif" :key="gif" />
    </div>
  </div>
</template>

<script>
import { value } from "vue-function-api";
const API_URL =
  "https://api.giphy.com/v1/gifs/search?api_key=JnxTmEGKXjZeUKBzRjTQoMDg8OX8pS5U&rating=pg&q=";
export default {
  setup() {
    const search = value("");
    const loading = value(false);
    const gifs = value([]);

    const searchGifs = async () => {
      loading.value = true;
      const response = await fetch(API_URL + search.value);
      const json = await response.json();
      gifs.value = json.data.map(gif => gif.images.fixed_height.url);
      setTimeout(() => {
        loading.value = false;
      }, 2000);
    };

    return {
      search,
      searchGifs,
      loading,
      gifs
    };
  }
};
</script>

<style>
.images {
  column-count: 4;
}

img {
  width: 100%;
}

.loading {
  margin: 0 auto;
}
</style>
