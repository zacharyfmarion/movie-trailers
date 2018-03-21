<template>
  <Flex class="video-detail" auto column>
    <div class="video-container">
      <iframe :src="url" type="text/html" width="853" height="480" frameborder="0" allowfullscreen></iframe>
    </div>
    <div class="video-detail__info">
      <h3>{{video.snippet.title}}</h3>
      <div>{{video.snippet.description}}</div>
    </div>
    <hr class="video-detail__separator" />
    <ImdbInfo class="video-detail__imdb" :movie="imdbData" />
  </Flex>
</template>

<script>
  import Flex from './Flex';
  import ImdbInfo from './ImdbInfo';

  export default {
    props: {
      video: {
        type: Object,
        required: true,
      },
      imdbData: {
        type: Object,
        required: true,
      }
    },
    computed: {
      url() {
        return `https://www.youtube.com/embed/${this.video.id.videoId}`;
      }
    },
    components: {
      Flex,
      ImdbInfo,
    } 
  }
</script>

<style>
  .video-container {
    overflow:hidden;
    padding-bottom:56.25%;
    position:relative;
    height:0; 
  }
  .video-container iframe,
  .video-container object,
  .video-container embed {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .video-detail__info {
    text-align: left;
    padding: 10px 0;
  }
  .video-detail__info > h3 {
    margin: 5px 0;
  }
  .video-detail__separator {
    border: none;
    border-top: 1px solid lightgray;
    width: 100%;
    height: 1px;
  }
  .video-detail__imdb {
    margin-top: 10px;
  }
</style>