<template>
  <div id="app">
    <p v-if="isLoading">Loading</p>
    <search v-on:SearchRequested="handleSearch"> </search>
    <p v-if="isTrending">Trending Gifs</p>
    <preview :gifs="gifs"> </preview>
  </div>
</template>

<script>
import Search from './components/Search.vue';
import Preview from './components/Preview.vue';

export default {
  components: { Search , Preview },
  name: 'App',
  data() {
    return {
      gifs : [],
      isLoading : true,
      isTrending: true,
    }
  },
  methods : {
    handleSearch(query) {
      this.gifs = [];
      this.isLoading = true;
      fetch(`http://api.giphy.com/v1/gifs/search?q=${query}&api_key=2zXWaoCrNcec2xuSwQ8MVt4AXImy8nWV`)
      .then((res) =>  { return res.json()})
      .then((res) => {
        this.gifs = res.data; 
        this.isLoading = false;
        this.isTrending= false; })
  }
    
  },
  created() {
    fetch('http://api.giphy.com/v1/gifs/trending?api_key=2zXWaoCrNcec2xuSwQ8MVt4AXImy8nWV')
    .then((res) =>  { return res.json()})
    .then((res) => {
       this.gifs = res.data; 
       this.isLoading = false; })
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
p{
  margin-left: 60px;
}
</style>
