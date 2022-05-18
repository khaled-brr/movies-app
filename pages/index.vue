<template>
  <div class="home">
    <hero />
    <div class="movies-card-container">
      <ul v-for="movie in movies" :key="movie.id">
        <li class="movies-card">
          <img :src="movie.logo_path" alt="" />
          <div>{{ movie.id }}</div>
          <div>{{ movie.name }}</div>
          <div>{{ movie.origin_country }}</div>
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
    }
  },

  async fetch() {
    await this.getMovies()
  },
  methods: {
    async getMovies() {
      const moviesData = axios.get(
        'https://api.themoviedb.org/3/movie/550?api_key=7d8ae35610ceed0a5fe301bc5d182678'
      )
      const res = await moviesData
      this.movies = res.data.results

    },
  },
}
</script>
