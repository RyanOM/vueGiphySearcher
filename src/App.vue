<template>
  <div id="app">
    <h1>Awesome Vuejs Gif Searcher</h1>
    <input v-model="searchTerm" v-on:keyup.enter="getGifs()" type="text" name="" value="">
    <button type="button" name="button" class="button" @click=getGifs()>Search</button>
    <hr>

    <div class="ui grid container">
      <div class="" v-for="gif in gifs" :key="gif.id">
        <div class="four wide column">
      <div class="ui card" style="margin-top: 20px">
        <div class="image"><img :src="gif"></div>
        <br>
        <div class="extra content">
          <div class="ui two buttons">
            <a class="ui basic green button" :href="gif">Open Gif</a>
            <button class="ui basic red button" v-clipboard:copy="gif">Copy URL</button>
          </div>
        </div>
        <br>
      </div>
      </div>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  data() {
    return {
      searchTerm: "",
      gifs: []
    }
  },
  methods: {
    getGifs() {
      let apiKey = "VcHdtRwzxX3JIpmb2rZvyKfumVlDygZa";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 12;

      let url = `${searchEndPoint}&api_key=${apiKey}&q=${
        this.searchTerm
      }&limit=${limit}`;

      fetch(url).then(response => {
        return response.json();
      })
      .then(json => {
        this.buildGifs(json);
      })
      .catch(err => {
        console.log(err);
      });
    },
    buildGifs(json) {
      console.log(json);
      this.gifs = json.data
        .map(gif => gif.id)
        .map(gifId => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input {
  padding: 5px;
  margin-bottom: 20px;
}

.button {
  background-color: rgb(0,172,0);
  color: white;
  padding: 5px 20px;
  border: none;
  display: block;
  margin: 0 auto;
}

.button:hover {
  background-color: rgb(0,18,0);
}

.gif-container {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

</style>
