<template>
  <main>
    <AppLoader v-if="$fetchState.pending" />

    <div v-else class="container single-movie">
      <nuxt-link to="/" class="button">Back</nuxt-link>

      <div class="movie-info">
        <div class="movie-img">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${movieDetail.poster_path}`"
            :alt="movieDetail.title"
          />
        </div>

        <div class="movie-content">
          <h1>{{ movieDetail.title }}</h1>
          <p v-if="movieDetail.tagline" class="movie-fact tagline">
            "{{ movieDetail.tagline }}"
          </p>

          <p class="movie-fact">
            {{
              new Date(movieDetail.release_date).toLocaleString('en-gb', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>

          <!-- genre -->
          <p class="movie-fact">
            <span v-for="genre in movieDetail.genres" :key="genre.id">
              {{ genre.name }}
            </span>
          </p>

          <p class="movie-fact">{{ movieDetail.runtime }} minutes.</p>

          <p class="movie-fact">
            Total Revenue:
            <span v-if="movieDetail.revenue > 0">
              {{
                movieDetail.revenue.toLocaleString('en-us', {
                  style: 'currency',
                  currency: 'USD',
                })
              }}
            </span>
            <span v-else> N/A</span>
          </p>

          <p class="movie-fact">
            {{ movieDetail.overview }}
          </p>

          <p v-if="movieDetail.production_companies.length" class="movie-fact">
            <span
              v-for="company in movieDetail.production_companies"
              :key="company.id"
            >
              {{ company.name }}
            </span>
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SingleMovie',

  data() {
    return {
      movieDetail: null,
    };
  },

  async fetch() {
    await this.getSingleMovie();
  },

  fetchDelay: 1500,

  methods: {
    async getSingleMovie() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=7d21bbfa3334a3920e67d193b9fbd3a2&language=en-US`
      );

      this.movieDetail = data.data;
    },
  },
};
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;

    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }

    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 36px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 16px;
        line-height: 1.5;

        span {
          font-weight: 600;
          display: inline-block;
          margin-right: 12px;
          color: #c92502;
        }
      }

      .tagline {
        font-style: italic;

        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
