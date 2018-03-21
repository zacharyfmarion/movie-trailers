<template>
  <Flex class="movie">
    <img class="movie-poster" :src="movie.Poster" alt="movie poster">
    <Flex column>
      <Flex v-for="attribute in attributes" :key="attribute">
        <div class="movie-attribute">{{attribute}}: </div>
        <strong>{{movie[attribute]}}</strong>
      </Flex>
      <Flex>
        <div class="movie-attribute">IMDB Rating: </div>
        <el-rate
          v-model="rating"
          :max="10"
          disabled
          allow-half
          text-color="#ff9900">
        </el-rate>
        <div class="movie-rating">{{rating}} / 10.0</div>
      </Flex>
      <Flex>
        <div class="movie-attribute">Actors: </div>
        <el-tag class="actor-tag" v-for="actor in actors" :key="actor">{{actor}}</el-tag>
      </Flex>
    </Flex>
  </Flex>
</template>

<script>
  import Flex from './Flex' ;
  export default {
    props: {
      movie: {
        type: Object,
        required: true,
      }
    },
    data: function() {
      return {
        attributes: ['Director', 'Production', 'Rated', 'Released', 'Runtime'],
      } 
    },
    computed: {
      actors() {
        return this.movie.Actors.split(', ');
      },
      rating() {
        return parseFloat(this.movie.imdbRating);
      }
    },
    components: {
      Flex,
    }
  }
</script>

<style>
  .movie-poster {
    height: 200px;
    margin-right: 20px;
  }
  .movie-attribute {
    margin-right: 5px;
  }
  .actor-tag {
    margin-right: 5px;
  }  
</style>