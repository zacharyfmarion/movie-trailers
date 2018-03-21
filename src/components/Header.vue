<template>
  <Flex class="header-wrapper" align="center">
    <h3 class="header-title">{{title}}</h3> 
    <SearchInput placeholder="Search for a movie..." :value="term" :onChange="onTermChange" :onEnter="onSearch" />
    <el-button class="search-button" @click="onSearch">Search Trailers</el-button>
  </Flex>
</template>

<script>
  import Flex from './Flex';
  import SearchInput from './SearchInput';

  export default {
    props: {
      title: {
        type: String,
        required: true,
      },
      term: {
        type: String,
        required: true,
      },
      onTermChange: {
        type: Function,
        required: true,
      },
      onSearch: {
        type: Function,
        required: true,
      }
    },
    components: {
      Flex,
      SearchInput,
    }
  }
</script>

<style>
  .header-wrapper {
    padding: 15px;
    background: #ffee58;
  }
  .header-title {
    display: flex;
    margin: 0;
    margin-right: 15px;
    white-space:nowrap;
  }
  .search-button {
    margin-left: 5px;
  }
  @media only screen and (max-width : 768px) {
    .search-button {
      display: none;
    }
  }
  .flex-input {
    display: flex;
    flex: 1 1 auto;
  }
</style>