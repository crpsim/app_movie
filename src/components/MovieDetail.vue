<template>
    <div v-if="isLoading">
      Chargement des détails...
    </div>
    <div v-else-if="movie">
      <div class="movie-details">
        <img :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`" alt="Poster" v-if="movie.poster_path">
        <h2>{{ movie.title }}</h2>
        <p>{{ movie.overview }}</p>
        <p><strong>Date de sortie :</strong> {{ movie.release_date }}</p>
        <p><strong>Genres :</strong> {{ genres }}</p>
        <p><strong>Durée :</strong> {{ movie.runtime }} minutes</p>
        <p><strong>Note moyenne :</strong> {{ movie.vote_average }} ({{ movie.vote_count }} votes)</p>
      </div>
    </div>
    <div v-else>
      Film non trouvé.
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import config from '@/config.json';
  
  export default {
    props: {
      id: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        isLoading: true,
        movie: null,
      };
    },
    computed: {
      genres() {
        return this.movie.genres.map(genre => genre.name).join(', ');
      },
    },
    async mounted() {
      await this.fetchMovie();
    },
    methods: {
      async fetchMovie() {
        this.isLoading = true;
        try {
          const response = await axios.get(`${config.url.movie_detail}${this.id}?language=fr-FR&api_key=${config.api_keyb}`);
          this.movie = response.data;
        } catch (error) {
          console.error(error);
        } finally {
          this.isLoading = false;
        }
      },
    },
  };
  </script>
  


  <style>
  .movie-details {
    background: white;
    padding: 20px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .movie-details img {
    margin: auto;
    max-width: 20%;
    height: auto;
    border-radius: 10px;
  }
  </style>