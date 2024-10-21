<template>
  <div id="app">
    <header>
      <h1>News App</h1>
      <input v-model="searchQuery" placeholder="Search news..." />
    </header>

    <NewsList 
      :news="filteredNews" 
      @newsClicked="handleNewsClick" 
    />

    <ReadNews :readNews="readNews" />
  </div>
</template>

<script>
import axios from 'axios';
import NewsList from './components/NewsList.vue';
import ReadNews from './components/ReadNews.vue';

export default {
  data() {
    return {
      news: [],             
      searchQuery: '',     
      readNews: []          
    };
  },
  components: {
    NewsList,
    ReadNews
  },
  mounted() {
    this.fetchNews();       
    this.loadReadNews();    
  },
  computed: {
    filteredNews() {
      return this.news.filter(article =>
        article.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  methods: {
    fetchNews() {
      const apiKey = '7e77e1bc64324333be98ad68f2f02690';
      const url = `https://newsapi.org/v2/top-headlines?country=us&apiKey=${apiKey}`;

      axios.get(url).then(response => {
        this.news = response.data.articles.map(article => ({
          sourceName: article.source.name,
          author: article.author,
          title: article.title,
          description: article.description,
          url: article.url,
          urlToImage: article.urlToImage,
          publishedAt: this.formatDate(article.publishedAt)
        }));
      }).catch(error => {
        console.error("Error fetching news:", error);
      });
    },
    handleNewsClick(newsItem) {
      this.readNews.push(newsItem);
      localStorage.setItem('readNews', JSON.stringify(this.readNews));
      window.open(newsItem.url, '_blank'); 
    },
    loadReadNews() {
      const storedReadNews = localStorage.getItem('readNews');
      if (storedReadNews) {
        this.readNews = JSON.parse(storedReadNews);
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      const options = { weekday: 'short', day: 'numeric', month: 'long', hour: '2-digit', minute: '2-digit' };
      return date.toLocaleDateString('en-US', options).replace(',', '').replace('AM', '').replace('PM', '');
    }
  }
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #f5f5f5;
}

input {
  padding: 10px;
  font-size: 16px;
}

h1 {
  font-size: 24px;
}
</style>
