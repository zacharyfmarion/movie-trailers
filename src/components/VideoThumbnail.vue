<template>
  <Flex class="thumbnail" :class="{'thumbnail-active': active}" @click.native="onClick(video)">
    <img :src="thumbnail.url" class="thumbnail-image" :style="{height: `${thumbnail.height}px`, minWidth: `${thumbnail.width}px`}" alt="thumbnail" />
    <Flex column class="thumbnail-info">
      <h4 class="thumbnail-title">{{video.snippet.title}}</h4> 
      <span class="thumbnail-channel">{{video.snippet.channelTitle}}</span>
    </Flex>
  </Flex>
</template>

<script>
  import Flex from './Flex';

  export default {
    props: {
      video: {
        type: Object,
      },
      onClick: {
        type: Function,
        default: function() {
          return () => {};
        }
      },
      active: {
        type: Boolean,
        default: false,
      },
      quality: {
        type: String,
        default: 'default',
        validator: value => {
          return ['default', 'high', 'medium'].includes(value);
        }
      }
    },
    computed: {
      thumbnail() {
        return this.video.snippet.thumbnails[this.quality];
      }
    },
    components: {
      Flex,
    }
  }
</script>

<style>
  .thumbnail {
    max-width: 400px;
    text-align: left;
    border-radius: 2px;
  }
  .thumbnail-active, .thumbnail:hover {
    background-color: #c6e2ff;
    cursor: pointer;
  }
  .thumbnail-image {
    border-radius: 2px;
    margin-right: 5px;
  }
  .thumbnail-info {
    padding: 5px;
    word-wrap: break-word;
    white-space: pre-wrap;
    word-break: break-all;
  }
  .thumbnail-title {
    margin: 0;
  } 
</style>