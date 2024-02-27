<template>
  <div>
    <MovieSearch @updateSearchTerm="updateSearchTerm"></MovieSearch>
    <MovieList @onSelectFilm="displayMovieDetails" :searchQuery="searchQuery"></MovieList>

    <v-dialog v-model="isDialogOpen">
      <template v-if="isDialogOpen">
        <MovieDetail @closeMovieDetail="hideMovieDetail" :id="currentFilmId"></MovieDetail>
      </template>
    </v-dialog>
  </div>
</template>

<script>
import MovieDetail from '@/components/MovieDetail.vue';
import MovieList from '@/components/MovieList.vue';
import MovieSearch from '@/components/MovieSearch.vue';

export default {
  name: 'HomeView',
  components: { MovieList, MovieDetail, MovieSearch },
  data() {
    return {
      isDialogOpen: false,
      currentFilmId: null,
      searchQuery: "",
    };
  },
  methods: {
    displayMovieDetails(filmId) {
      this.currentFilmId = filmId;
      this.isDialogOpen = true; 
    },
    hideMovieDetail() {
      this.isDialogOpen = false;
    },
    updateSearchTerm(query) {
      this.searchQuery = query;
    },
  },
}
</script>
