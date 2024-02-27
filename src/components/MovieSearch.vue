<template>
    <div class="search-container">
      <label for="movieSearchInput" class="search-label">Search Movie:</label>
      <input id="movieSearchInput" v-model="searchTerm" type="text" class="search-input">
    </div>
  </template>
  
  <script>
  export default {
    name: 'SearchMovieInput',
    data() {
      return {
        searchTerm: "", 
      };
    },
    watch: {
      searchTerm(newValue) {
        this.$emit('updateSearchTerm', newValue);
      },
    },
  }
  </script>
  
  <style scoped>
  .search-container {
    margin: auto;
    border: 2px solid #000;
    width: 220px; 
    padding: 10px; 
    box-sizing: border-box; 
    text-align: center;
  }
  
  .search-label {
    display: block; 
    margin-bottom: 5px; 
  }
  
  .search-input {
    width: 100%; 
    background-color: #afeeee;
    border: 1px solid #ccc;
    padding: 5px;
    box-sizing: border-box; 
  }
  </style>