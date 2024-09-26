<template>
    <div>
      <h2 v-if="selectedMovie" class="selected-movie">Película: {{ selectedMovie }}</h2>
      <table>
        <caption>Películas</caption>
        <thead>
          <tr>
            <th>Título</th>
            <th>Calificación</th>
            <th>IMDb</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="movie in movies" :key="movie.id" @click="selectMovie(movie.movie)">
            <td>{{ movie.movie }}</td>
            <td>{{ movie.rating }}</td>
            <td><a :href="movie.imdb_url" target="_blank">Ver en IMDb</a></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, onMounted } from 'vue';
  
  interface Movie {
    id: number;
    movie: string;
    rating: number;
    imdb_url: string;
  }
  
  export default defineComponent({
    emits: ['movie-selected'],
    setup(_, { emit }) {
      const movies = ref<Movie[]>([]);
      const selectedMovie = ref<string>('');
  
      const fetchMovies = async () => {
        try {
          const response = await fetch('https://dummyapi.online/api/movies');
          const data: Movie[] = await response.json();
          movies.value = data;
        } catch (error) {
          console.error('Error al obtener las películas:', error);
        }
      };
  
      const selectMovie = (title: string) => {
        selectedMovie.value = title;
        emit('movie-selected', title);
      };
  
      onMounted(() => {
        fetchMovies();
      });
  
      return {
        movies,
        selectMovie,
        selectedMovie,
      };
    },
  });
  </script>
  
  <style scoped>
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    border: 1px solid #ccc;
    padding: 8px;
    text-align: left;
  }
  
  tr:hover {
    background-color: #f5f5f5;
  }
  
  .selected-movie {
    text-align: center;
    margin-bottom: 15px;
    font-size: 20px;
  }
  
  caption {
    font-size: 30px;
    margin-bottom: 10px;
  }
  </style>
  