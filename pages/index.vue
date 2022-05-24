<template>
  <div class="home text-danger">
    <hero />

    <div class="container">
      <div class="search-movies">
        <input
          @keyup.enter="$fetch"
          type="text"
          placeholder="search in movies"
          v-model.lazy="shearchMoviesWord"
          name=""
          id=""
        />
        <button
          v-show="shearchMoviesWord !== ''"
          @click="shearchMoviesWord = ''"
          class="button clear-search"
        >
          clear search
        </button>
      </div>
      <ul class="movies-card-container">
        <li class="movies-card" v-for="movie in movies" :key="movie.id">
          <div class="img-about-container">
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
            <div class="about-movie mt-3">{{ movie.overview }}</div>
          </div>
          <div style="color: #fff">{{ movie.id }}</div>
          <div style="color: #fff">
            {{
              new Date(movie.release_date).toLocaleString('en', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </div>

          <NuxtLink
            class="button button-green"
            style="color: #fff"
            :to="{ name: 'movies-movieId', params: { movieId: movie.id } }"
            >see more about movies
          </NuxtLink>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      movies: [],
      shearchMovies: [],
      shearchMoviesWord: '',
    }
  },

  async fetch() {
    if (this.shearchMoviesWord === '') {
      await this.getMovies()
      return
    }
    await this.getSearchedMovies()
   },
  methods: {
    async getMovies() {
      const moviesData = axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1'
      )
      const res = await moviesData
      this.movies = res.data.results
    },
    async getSearchedMovies() {
      const moviesData = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1&query=${this.shearchMoviesWord}`
      )
      const res = await moviesData
      console.log('llll')

      this.shearchMovies = res.data.results
    },
  },
}
</script>

<style scoped>
.search-movies {
  margin-top: 40px;
}
.search-movies button {
  border-radius: 0;
  margin: 0;
  padding: 8px 20px !important;
  font-size: 20px;
}
.search-movies input {
  margin: 0;
  padding: 8px 20px;
  outline: none;
  font-size: 18px;
}
.movies-card-container {
  display: flex;
  gap: 2%;
  flex-wrap: wrap;
  list-style: none;
}
.movies-card {
  width: calc(96% / 3);
  color: #fff;
  margin: 30px 0;
  height: 530px;
}
.img-about-container {
  widows: 100%;
  height: 430px;
}

.img-about-container img {
  width: 100%;
  height: 100%;
}
</style>
