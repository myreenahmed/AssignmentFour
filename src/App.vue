<script setup>
import axios from "axios";
import { ref } from "vue";

const apiKey = import.meta.env.VITE_TMDB_API_KEY;
const selectedMovie = ref(null);
const movieDetails = ref(null);

const getMovies = async () => {
  const result = await axios.get(
    `https://api.themoviedb.org/3/movie/${selectedMovie.value}?api_key=${apiKey}&language=en-US&append_to_response=videos`
  );
  console.log(result);
  movieDetails.value = result.data;
};
</script>

<template>
  <div>
    <form>
      <select v-model="selectedMovie">
        <option value="550">Fight Club</option>
        <option value="680">Pulp Fiction</option>
        <option value="218">The Terminator</option>
        <option value="629">The Usual Suspects</option>
        <option value="769">Goodfellas</option>
        <option value="424">Schindler's List</option>
        <option value="278">The Shawshank Redemption</option>
        <option value="155">The Dark Knight</option>
        <option value="274">The Silence of the Lambs</option>
        <option value="272">Batman Begins</option>
      </select>
      <button type="button" @click="getMovies">Get</button>
    </form>
    <div v-if="movieDetails">
      <h2>{{ movieDetails.title }}</h2>
      <p> {{ movieDetails.overview }}</p>
      <p>Release date: {{ movieDetails.release_date }}</p>
      <p>Runtime: {{ movieDetails.runtime }} minutes</p>
      <p>Popularity: {{ movieDetails.popularity}}</p>
      <p>Vote average: {{ movieDetails.vote_average }}</p>
      <p>Vote Count: {{ movieDetails.vote_count }}</p>
      <p>Budget: {{ movieDetails.budget }}</p>
      <p>Revenue: {{ movieDetails.revenue }}</p>
      <img
        :src="`https://image.tmdb.org/t/p/w500${movieDetails.poster_path}`"
      />
      <iframe
        :src="`https://www.youtube.com/embed/${
          movieDetails.videos.results
            .filter((trailer) => trailer.type === 'Trailer')
            .at(0).key
        }`"
        frameborder="1"
      ></iframe>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

* {
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

form {
  display: flex;
  justify-content: center;
  margin: 2rem auto;
}

label {
  margin-bottom: 1rem;
}

select {
  padding: 1rem;
  font-size: 1.2rem;
  border-radius: 2px;
  margin-right: 1rem;
  background-color: #f0f0f0;
  color: #333;
  font-weight: 500;
  font-family: "Poppins", sans-serif;
}

button {
  padding: 1rem 2rem;
  font-size: 1.2rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #ff7eb9;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
  font-weight: 500;
  font-family: "Poppins", sans-serif;
}

button:hover {
  background-color: #ff66a3;
}

#movieDetails {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 2rem auto;
  max-width: 800px;
}

h2 {
  font-size: 3rem;
  margin-top: 0;
  text-align: center;
  color: #ff7eb9;
  font-weight: 600;
  font-family: "Poppins", sans-serif;
}

p {
  font-size: 1.4rem;
  margin: 1rem;
  text-align: justify;
  line-height: 1.5;
  color: #333;
  text-align: center;
  font-weight: 400;
  font-family: "Poppins", sans-serif;
}

img {
  display: block;
  margin: 2rem auto;
  max-width: 100%;
  height: auto;
  justify-content: center;
  
}

iframe {
  justify-content: center;
  display: block;
  align-items: center;
}
</style>
