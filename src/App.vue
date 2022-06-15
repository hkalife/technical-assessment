<script>
import axios from "axios";
import { defineComponent, onMounted, reactive, watch } from "vue";

export default defineComponent ({
  name: 'App',

  setup: () => {

    const data = reactive({
      allFilms: [],
      searchTextMovie: '',
      filmsToShow: [],
      sortType: '',
    })

    const getMovies = async () => {
      const url = "https://the-one-api.dev/v2/movie";
      const apiToken = "JSxN_kZPm24SKg8uRnPw";
      axios
        .get(url, {
          headers: {
            Authorization: `Bearer ${apiToken}`,
          },
        })
        .then((response) => {
          console.log(response.data);
          data.filmsToShow = response.data?.docs;
          data.allFilms = data.filmsToShow;
        });
    }

    onMounted(() => {
      getMovies();
    });

    watch(data.searchTextMovie, () => {
      console.log('watch data');
      data.filmsToShow = filmsToShow.filter(film => film.name === searchTextMovie);
    })

    return {
      data,
    }
  }
});
</script>

<template>
  <div class="challenge">
    <div id="subheader-movies">
      <div class="title-info">
        <h1>Lord of the Rings movies</h1>
        <p>Ave. movie runtime: xxx min</p>
        <p>Ave. movie budget: $XXM</p>
      </div>
      <div class="search-options">
        <input v-model="data.searchTextMovie" >
        <select v-model="data.sortType">
          <option>
            Sort by budget (asc)
          </option>
          <option>
            Sort by budget (desc)
          </option>
        </select>

        {{data.searchTextMovie}} - {{data.sortType}}
      </div>
    </div>
    <div id="list-movies">
      <div
        class="movie-card"
        v-for="(movie, index) in data.filmsToShow"
        :key="movie"
      >
        <div :id="`movie-poster-${index}`" class="movie-poster-section">
        </div>

        <div class="card-body">
          <h3>{{ movie.name }}</h3>
          <p>{{ movie.runtimeInMinutes }} min</p>
          <p>{{ movie.academyAwardWins }} wins and {{ movie.academyAwardNominations }} nominations</p>

          <div class="revenue-info">
            <div class="card-budget">
              <p>Budget</p>
              <p>{{ movie.budgetInMillions }}M</p>
            </div>
            <div class="card-revenue">
              <p>Revenue</p>
              <p>{{ movie.boxOfficeRevenueInMillions }}M</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@import "./assets/base.scss";
</style>
