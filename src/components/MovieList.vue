<template>
    <div class="film-gallery">
      <div class="film-row" v-for="film in filteredFilms" :key="film.id">
        <div class="film-column">
          <div class="film-card">
            <h2>{{ film.title }}</h2>
            <img :src="getImagePath(film.poster_path)" alt="Film Poster" class="film-image">
            <button @click="notifyParent(film.id)">Plus d'infos</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import config from '@/config.json';
  
  export default {
    name: 'FilmList',
    props: {
      searchQuery: String,
    },
    data() {
      return {
        films: [],
        basePath: '',
      };
    },
    computed: {
      filteredFilms() {
        if (!this.searchQuery) return this.films;
        return this.films.filter(film => film.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
      },
    },
    methods: {
      fetchFilms() {
        axios.get(config.url.movie_list, {
          headers: {
            Authorization: `Bearer ${config.api_key}`,
          },
        }).then(response => {
          this.basePath = config.url.photo_path;
          this.films = response.data.results;
        }).catch(error => console.error('Error fetching films:', error));
      },
      getImagePath(posterPath) {
        return `${this.basePath}${posterPath}`;
      },
      notifyParent(filmId) {
        this.$emit('onSelectFilm', filmId); 
      },
    },
    created() {
      this.fetchFilms();
    },
  }
  </script>
  
  
  <style>
  .film-gallery {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .film-column {
    flex: 0 1 31%; 
    margin-bottom: 20px;
  }
  .film-card {
    padding: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .film-image {
    width: 100%;
    height: auto;
  }
  button {
    display: block;
    width: 100%;
    padding: 10px;
    background-color: #007BFF;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }
  button:hover {
    background-color: #237fe2;
  }
  </style>