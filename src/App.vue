<template>
  <div id="app">
    <h1>TE - TWEB - Kevin Pradervand</h1>
    <p>Films {{offset}} to {{currentLast}}</p>
    <button v-on:click="goFurther">Go further</button>
    <button v-on:click="goBack">Go back</button>
    <ul v-if="posts && posts.length">
      <li v-for="post of posts"
          v-bind:key="post.id">
          <h1>{{post.title}}</h1>
          <p>{{splitDate(post.release_date)}}</p>
          <img :src="'https://image.tmdb.org/t/p/w500/' + post.poster_path">
      </li>
    </ul>

    <button v-on:click="goFurther">Go further</button>
    <button v-on:click="goBack">Go back</button>
    <p>Films {{offset}} to {{currentLast}}</p>
  </div>
</template>

<script>
import axios from 'axios';
import Vue from 'vue';

/*
url1 pour le local : http://localhost:5000/movies?limit=
*/

let url1 = 'https://twebte2backend.herokuapp.com/movies?limit=';
let url2 = '&offset=';
let urlPoster = 'https://image.tmdb.org/t/p/w500/';
let filmByPage = 20;

export default {

  name: 'app',
  data () {
    return {
      posts: [],
      currentLast: 20,
      offset: 0
    }
  },

  created(){
    axios.get(url1+filmByPage+url2+this.offset)
    .then(response => {
      this.posts = response.data.movieResult
    })
  },
  methods: {
    goFurther: function (){
      if (this.currentLast == 10000){
      }else{
        this.currentLast = this.currentLast + 20;
        this.offset = this.offset + 20;
        window.scrollTo(top);
        axios.get(url1+filmByPage+url2+this.offset)
        .then(response => {
          this.posts = response.data.movieResult
        })
      }
    },
    goBack: function (){
      if (this.offset == 0){
      }else{
        this.currentLast = this.currentLast - 20;
        this.offset = this.offset - 20;
        window.scrollTo(top);
        axios.get(url1+filmByPage+url2+this.offset)
        .then(response => {
          this.posts = response.data.movieResult
        })
      }
      
    },
    splitDate: function (date){
      date = date.split("T");
      return date[0];
    }
  }
}


</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
