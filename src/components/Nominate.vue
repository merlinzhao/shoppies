<template>
  <div class="nominate-bg">
    <div class="center">
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
            :movieID="item.imdbID"
            @nominate-data="nominateMovie"
          />
        </div>
      </div>
      <div class="show-nominations">
        <p v-for="movie in userNominations" :key="movie">movie</p>
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
      userNominations: {},
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
            "&type=movie&r=json&s=" +
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
    nominateMovie(e) {
      if (e.ID in this.userNominations) {
        console.log("already nominated");
        return;
      }
      this.userNominations[e.ID] = e.ID;
      console.log(this.userNominations);

      this.$emit("user-nominated", this.userNominations);
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
.center {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card {
  background: #ddd;
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
  width: 450px;
  height: 80px;
  line-height: 80px;
  font-size: 25px;
}
label {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  color: #aaaaaa;
  transition: 0.2s all;
  cursor: text;
  font-weight: 700;
}
input {
  width: 100%;
  height: 25px;
  border: 0;
  outline: 0;
  padding: 0.5rem 0;
  border-bottom: 2px solid #838383;
  box-shadow: none;
  color: #111;
}

input:focus,
input:valid {
  border-color: #00dd22;
}
input:focus ~ label,
input:valid ~ label {
  font-size: 16px;
  top: -33px;
  color: #00dd22;
}
</style>