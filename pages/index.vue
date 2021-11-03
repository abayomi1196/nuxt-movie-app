<template>
  <main>
    <AppHero />

    <!-- searchBar -->
    <div class="container search">
      <input
        v-model.lazy="searchTerm"
        type="text"
        placeholder="Search Nuxt Movie App.."
        @keyup.enter="$fetch"
      />

      <button v-show="searchTerm !== ''" class="button" @click="clearSearch">
        Clear Search
      </button>
    </div>
    <!-- end of searchBar -->

    <AppLoader v-if="$fetchState.pending" />

    <p v-else-if="$fetchState.error" class="error-text">
      <span>An error occurred :(</span>
    </p>

    <AppMoviesGrid
      v-else
      :movies="movies"
      :searched-movies="searchedMovies"
      :search-input="searchTerm"
    />
  </main>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchTerm: '',
    };
  },

  async fetch() {
    if (this.searchTerm === '') {
      await this.getMovies();
      return;
    }

    if (this.searchTerm !== '') {
      await this.searchMovies();
    }
  },
  head() {
    return {
      title: 'Movie App - Latest In Cinemas & Online',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, streaming, cinemas, netflix, hbo',
        },
      ],
    };
  },

  fetchDelay: 1000,

  methods: {
    async getMovies() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=7d21bbfa3334a3920e67d193b9fbd3a2&language=en-US&page=1`
      );

      const result = await data;

      this.movies = result.data.results;
    },

    async searchMovies() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=7d21bbfa3334a3920e67d193b9fbd3a2&language=en-US&page=1&query=${this.searchTerm}&include_adult=true`
      );

      const result = await data;

      this.searchedMovies = result.data.results;
    },

    clearSearch() {
      this.searchTerm = '';
      this.searchedMovies = [];
    },
  },
};
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  padding: 32px 16px;

  input {
    max-width: 350px;
    width: 100%;
    padding: 12px 6px;
    font-size: 14px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  .button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
</style>
