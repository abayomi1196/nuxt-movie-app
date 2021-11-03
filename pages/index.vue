<template>
  <main>
    <AppHero />

    <AppMoviesGrid :movies="movies" />
  </main>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      movies: [],
    };
  },

  async fetch() {
    await this.getMovies();
  },

  methods: {
    async getMovies() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=${process.env.API_KEY}&language=en&page=1`
      );

      const result = await data;

      this.movies = result.data.results;
    },
  },
};
</script>
