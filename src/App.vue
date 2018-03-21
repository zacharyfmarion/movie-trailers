<template>
  <div id="app">
    <Header id="app-header" title="Api Mashup" :term="term" :onTermChange="handleTermChange" :onSearch="getData" />
    <Flex id="app-body">
      <Flex auto v-if="loaded">
        <Flex id="app-main" auto>
          <Flex column auto v-if="loaded" >
            <MovieDetail :video="activeVideo" :imdbData="imdbData" />
          </Flex>
        </Flex>
        <Flex id="app-sidebar" column>
          <VideoThumbnail 
            v-for="video in videos" 
            class="app-thumbnail" 
            :key="video.etag" 
            :video="video" 
            :onClick="changeVideo"
            :active="activeVideo.etag === video.etag"
          />
        </Flex>
      </Flex>
      <UpcomingMovies id="upcoming-movies" :onMovieClick="handleUpcomingMovieClick" v-else/>
    </Flex>
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import Flex from './components/Flex.vue'
  import MovieDetail from './components/MovieDetail.vue'
  import VideoThumbnail from './components/VideoThumbnail.vue'
  import UpcomingMovies from './components/UpcomingMovies.vue'
  // package imports
  import axios from 'axios';
  // local imports
  import { YOUTUBE_KEY, OMDB_KEY, TMDB_KEY } from './constants';

  export default {
    name: 'app',
    data: function() {
      return {
        term: '',
        videos: [],
        activeVideo: null,
        loaded: false,
        imdbData: null,
        tmdbData: null,
      }
    },
    created: function() {
      this.getVideos();
    },
    methods: {
      getVideos() {
        const params = {
          part: 'snippet',
          key: YOUTUBE_KEY,
          q: this.term + ' trailer',
          maxResults: 15,
          type: 'video'
        };
        return axios.get(`https://www.googleapis.com/youtube/v3/search`, { params });
      },
      getImdbData() {
        return axios.get(`http://www.omdbapi.com/?apikey=${OMDB_KEY}&r=json&t=${this.term}`);
      },
      getTmdbData() {
        return axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${TMDB_KEY}&query=${this.term}`)
      },
      // make requests to both API endpoints
      getData() {
        if (!this.term || this.term === '') return;
        Promise.all([this.getVideos(), this.getImdbData(), this.getTmdbData()])
          .then(([youtubeData, imdbData, tmdbData]) => {
            this.videos = youtubeData.data.items;
            this.activeVideo = this.videos[0];
            this.imdbData = imdbData.data;
            this.tmdbData = tmdbData.data.results[0];
            this.loaded = true;
          })
      },
      changeVideo(video) {
        this.activeVideo = video;
      },
      handleTermChange(e) {
        console.log(e);
        this.term = e.target.value;
      },
      handleUpcomingMovieClick(title) {
        this.term = title;
        this.getData();
      }
    },
    components: {
      Header,
      Flex,
      VideoThumbnail,
      UpcomingMovies,
      MovieDetail,
    }
  }
</script>

<style>
  body {
    margin: 0;
  }
  #app-header {
    position: fixed;
    left: 0;
    right:0;
    top: 0;
    z-index: 10;
  }
  #app-body {
    margin-top: 70px;
  }
  #app-main {
    margin: 10px;
  }
  #app-sidebar {
    min-width: 400px;
    padding: 10px;
  }
  .app-thumbnail {
    margin-bottom: 8px;
  }
  #upcoming-movies {
    margin-top: 20px;
  }
  @media only screen and (max-width : 768px) {
    #upcoming-movies {
      margin-top: 0;
    }
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>
