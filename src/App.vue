<template>
  <div>
    <select v-model="selectedMovie">
      <option v-for="movieId in movieIds" :value="movieId">
        {{ movies[movieId].title }}
      </option>
    </select>
    <button @click="getMovieDetails">Get</button>
    <div v-if="movieDetails">
      <h1>{{ movieDetails.title }}</h1>
      <p>{{ movieDetails.overview }}</p>
      <p>Release date: {{ movieDetails.release_date }}</p>
      <p>Runtime: {{ movieDetails.runtime }} minutes</p>
      <p>Vote average: {{ movieDetails.vote_average }}</p>
      <p>Genres: {{ movieDetails.genres.map(genre => genre.name).join(', ') }}</p>
      <div v-if="movieDetails.videos.results.length">
        <p>Trailer:</p>
        <iframe :src="'https://www.youtube.com/embed/' + movieDetails.videos.results[0].key" width="560" height="315"></iframe>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      movieIds: [550, 680, 218, 629, 769, 424, 278, 155, 274, 272],
      movies: {},
      selectedMovie: null,
      movieDetails: null,
    };
  },
  mounted() {
    this.getMovies();
  },
  methods: {
    getMovies() {
      const promises = this.movieIds.map((movieId) => {
        return axios.get(`https://api.themoviedb.org/3/movie/${movieId}`, {
          params: {
            api_key: "83c9bffe98be4fb39b22d95d93999870",
            append_to_response: "videos",
          },
        });
      });
      Promise.all(promises)
        .then((responses) => {
          responses.forEach((response) => {
            this.movies[response.data.id] = response.data;
          });
        })
        .catch((error) => {
          console.error(error);
        });
    },
    getMovieDetails() {
      axios
        .get(`https://api.themoviedb.org/3/movie/${this.selectedMovie}`, {
          params: {
            api_key: "83c9bffe98be4fb39b22d95d93999870",
            append_to_response: "videos",
          },
        })
        .then((response) => {
          this.movieDetails = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
</style>

