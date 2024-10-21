<template>
  <div class="news-list">
    <div v-for="newsItem in news" :key="newsItem.url" class="news-item" @click="handleClick(newsItem)">
      <img :src="newsItem.urlToImage" alt="news image" />
      <h2>{{ newsItem.title }}</h2>
      <p><strong>Source:</strong> {{ newsItem.sourceName }}</p>
      <p><strong>Author:</strong> {{ newsItem.author }}</p>
      <p><strong>Description:</strong> {{ newsItem.description }}</p>
      <p><strong>Published At:</strong> {{ newsItem.publishedAt }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['news'],
  methods: {
    handleClick(newsItem) {
      this.$emit('newsClicked', newsItem);
    }
  }
};
</script>

<style scoped>
.news-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.news-item {
  cursor: pointer;
  border: 1px solid #ccc;
  padding: 10px;
  transition: transform 0.2s;
}

.news-item:hover {
  transform: scale(1.02);
}

.news-item img {
  width: 100%;
  height: auto;
}

h2 {
  font-size: 18px;
}

p {
  font-size: 14px;
}
</style>
