<template>
  <div class="home text-danger">
    <hero />
    <div class="container">
      <div class="search-movies ms-2">
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
          @click="clearSearch"
          class="button clear-search"
        >
          clear search
        </button>
      </div>
      <loading v-if="$fetchState.pending" />
      <div v-else class="movies-section">
        <!--Movies  -->
        <ul
          v-if="shearchMoviesWord === ''"
          class="all-movies movies-card-container"
        >
          <li class="movies-card mt-3" v-for="movie in movies" :key="movie.id">
            <div class="img-about-container">
              <img
                :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                alt=""
              />
              <div class="about-movie mt-3">{{ movie.overview }}</div>
              <div class="vote-movie">{{ movie.vote_average }}</div>
            </div>
            <!-- <div class="mt-3">{{ movie.id }}</div> -->
            <div class="movies-date mt-3">
              released:
              {{
                new Date(movie.release_date).toLocaleString('en', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </div>
            <div class="">{{ movie.title }}</div>

            <NuxtLink
              class="button button-green mt-3"
              style="color: #fff"
              :to="{ name: 'movies-movieId', params: { movieId: movie.id } }"
              >see more about movies
            </NuxtLink>
          </li>
        </ul>
        <!-- Searched Movies -->
        <ul v-else class="searched-movies movies-card-container">
          <li
            class="movies-card mt-3"
            v-for="movie in shearchMovies"
            :key="movie.id"
          >
            <div class="img-about-container">
              <img
                :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                alt=""
              />
              <div class="about-movie mt-3">{{ movie.overview }}</div>
              <div class="vote-movie">{{ movie.vote_average }}</div>
            </div>
            <!-- <div class="mt-3">{{ movie.id }}</div> -->
            <div class="movies-date mt-3">
              released:
              {{
                new Date(movie.release_date).toLocaleString('en', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </div>
            <div class="">{{ movie.title }}</div>

            <NuxtLink
              class="button button-green mt-3"
              style="color: #fff"
              :to="{ name: 'movies-movieId', params: { movieId: movie.id } }"
              >see more about movies
            </NuxtLink>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  head() {
      return {
        title: this.title,
        meta: [
          // hid is used as unique identifier. Do not use `vmid` for it as it will not work
          {
            hid: 'description',
            name: 'description',
            content: 'My custom description'
          }
        ]
      }
    },
  data() {
    return {
      movies: [],
      title: 'Movies Application',
      shearchMovies: [],
      shearchMoviesWord: '',
    }
  },

  async fetch() {
    if (this.shearchMoviesWord === '') {
      await this.getMovies()
      return
    }
    if (this.shearchMoviesWord !== '') {
      await this.getSearchedMovies()
    }
  },
  fetchDelay: 1000,

  methods: {
    async getMovies() {
      const moviesData = axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=7d8ae35610ceed0a5fe301bc5d182678&language=en-US&page=1'
      )

      const res = await moviesData
      this.movies = res.data.results
    },
    async getSearchedMovies() {
      const moviesData = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=7d8ae35610ceed0a5fe301bc5d18267&language=en-US&page=1&query=${this.shearchMoviesWord}`
      )
      const res = await moviesData
      this.shearchMovies = res.data.results
    },
    clearSearch() {
      this.shearchMoviesWord = ''
      this.shearchMovies = []
    },
  },
}
</script>

<style scoped>
.loading {
  padding-top: 120px;
  align-items: flex-start;
}
.ms-2 {
  margin-left: 2rem;
}
.mt-3 {
  margin-top: 2rem;
}
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
  padding: 30px;
  flex-wrap: wrap;
  list-style: none;
}
.movies-card {
  flex: 1 1 300px;
  color: #222;
  margin: 20px 0;
  height: 630px;
  padding-bottom: 10px;
  border-bottom: 2px solid #017374;
}
.img-about-container {
  widows: 100%;
  height: 430px;
  position: relative;
  overflow: hidden;
}
.vote-movie {
  position: absolute;
  top: 0;
  left: 0;
  width: 50px;
  height: 40px;
  background: #017374;
  padding: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  border-bottom-right-radius: 20px;
}
.img-about-container:hover .about-movie {
  transform: translateY(0);
}
.img-about-container .about-movie {
  transition: all 0.4s ease;
  position: absolute;
  background: #017374;
  color: #fff;
  padding: 20px;
  left: 0;
  width: 100%;
  bottom: 0;
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
  overflow: hidden;
  right: 0;
  transform: translateY(100%);
}
.img-about-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
</style>
