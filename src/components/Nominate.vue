<template>
  <div class="nominate-bg">
    <div class="input-field">
      <input
        v-model="search"
        type="text"
        id="movie-name"
        maxlength="128"
        required
      />
      <label for="movie-name">Search Movie</label>
    </div>
    <div class="row resultRow" style="background: green">
      <div class="col-3 card" v-for="item in movieResults" :key="item.imdbID">
        <MovieCard
          :title="item.Title"
          :year="item.Year"
          :posterLink="item.Poster"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MovieCard from "./MovieCard.vue";
export default {
  components: {
    MovieCard,
  },
  data() {
    return {
      search: "",
      awaitingSearch: false,
      inputField: "",
      apikey: "8b8c26c3",
      movieResults: {},
    };
  },
  mounted() {
    this.inputField = document.getElementById("movie-name");
  },
  methods: {
    fetchMovies() {
      axios
        .get(
          "https://www.omdbapi.com/?apikey=" +
            this.apikey +
            "&s=" +
            this.inputField.value
        )
        .then((response) => {
          let valid = response.data.Response;
          if (valid) {
            console.log(response.data);
            this.movieResults = response.data.Search;
          } else {
            console.log("fetch not valid");
          }
        })
        .catch(() => {
          console.log("ERROR FETCHING");
        });
    },
  },
  watch: {
    search: function () {
      if (!this.awaitingSearch) {
        setTimeout(() => {
          this.fetchMovies();
          this.awaitingSearch = false;
        }, 1200); // 1200ms delay
      }
      this.awaitingSearch = true;
    },
  },
};
</script>

<style scoped>
.nominate-bg {
  width: 100vw;
  height: 100vh;
  min-height: 800px;
  background: #222;
  margin: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card {
  background: blue;
}
.resultRow {
  display: flex;
  flex-direction: row;
  width: 90%;
  max-width: 1200px;
  overflow: auto;
}

.input-field {
  position: relative;
  width: 250px;
  height: 44px;
  line-height: 44px;
}
label {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  color: #d3d3d3;
  transition: 0.2s all;
  cursor: text;
  font-weight: 700;
}
input {
  width: 100%;
  border: 0;
  outline: 0;
  padding: 0.5rem 0;
  border-bottom: 2px solid #d3d3d3;
  box-shadow: none;
  color: #111;
}

input:focus,
input:valid {
  border-color: #00dd22;
}
input:focus ~ label,
input:valid ~ label {
  font-size: 14px;
  top: -24px;
  color: #00dd22;
}
</style>