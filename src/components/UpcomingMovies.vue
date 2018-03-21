<template>
  <Flex auto justify="start" wrap>
    <Flex 
      class="upcoming-movie" 
      auto column 
      align="center" 
      v-for="movie in topMovies" 
      @click.native="onMovieClick(movie.title)"
      :key="movie.id"
    >
      <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" alt="movie poster" />
      <h3>{{movie.title}}</h3>
    </Flex>
  </Flex>
</template>

<script>
  // displays upcoming movies gotten from TMDB 
  import Flex from './Flex';
  import axios from 'axios';
  import { TMDB_KEY } from '../constants';

  export default {
    props: {
      onMovieClick: {
        type: Function,
        default: function() {
          return () => {};
        }
      }
    },
    created: function() {
      this.getUpcomingMovies();
    },
    data: function() {
      return {
        movies: [],
      };
    },
    computed: {
      topMovies() {
        return this.movies.slice(0, 18);
      }
    },
    methods: {
      getUpcomingMovies() {
        const params = {
          api_key: TMDB_KEY,
          langauge: 'en-US',
          page: 1,
        };
        axios.get(`https://api.themoviedb.org/3/movie/upcoming`, { params }).then(res => {
          console.log(res);
          this.movies = res.data.results;
        })
      },
    },
    components: {
      Flex,
    }
  }
</script>

<style>
  .upcoming-movie {
    min-width: 0;
    flex-basis: 25%;
    flex-grow: 0;
  }
  .upcoming-movie:hover {
    cursor: pointer;
  }
  @media only screen and (max-width : 768px) {
    .upcoming-movie {
      flex-basis: 50%;
    }
  }
  @media only screen and (max-width : 320px) {
    .upcoming-movie {
      flex-basis: 100%;
    }
  }
  .upcoming-movie > img {
    width: 300px;
    max-width: 100%; 
    max-height: 500px; 
  }
</style>