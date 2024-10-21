<template>
  <div class="read-news">
    <h3>Read News</h3>
    <div v-for="newsItem in readNews" :key="newsItem.url" class="read-news-item">
      <img :src="newsItem.urlToImage" alt="news image" />
      <h4>{{ newsItem.title }}</h4>
      <a :href="newsItem.url" target="_blank">Read again</a>
    </div>
  </div>
</template>

<script>
export default {
  props: ['readNews']
};
</script>

<style scoped>
.read-news {
  margin-top: 20px;
}

.read-news-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.read-news-item img {
  width: 50px;
  height: 50px;
  margin-right: 10px;
}

h4 {
  font-size: 16px;
  margin: 0;
}

a {
  margin-left: 10px;
  color: blue;
  text-decoration: underline;
}
</style>
