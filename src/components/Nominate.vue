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
        <label for="movie-name">Search Films</label>
      </div>
      <div class="showResults">
        <div class="row resultRow">
          <div class="loader" v-if="loading"></div>
          <div class="col card" v-for="item in movieResults" :key="item.imdbID">
            <MovieCard
              v-if="notLoading"
              :title="item.Title"
              :year="item.Year"
              :posterLink="item.Poster"
              :movieID="item.imdbID"
              :userNom="userNominations"
              :type="'nominate'"
              @nominate-data="nominateMovie"
            />
          </div>
        </div>
      </div>
      <div class="show-nominations">
        <h1 class="your-head">Your 2020 Nominations</h1>
        <div class="row resultRow">
          <div
            class="col card"
            v-for="item in userNominations"
            :key="item.imdbID"
          >
            <MovieCard
              :title="item.Title"
              :year="item.Year"
              :posterLink="item.Poster"
              :movieID="item.ID"
              :userNom="userNominations"
              :type="'remove'"
              @remove-nominate="removeNomination"
            />
          </div>
        </div>
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
      loading: false,
      notLoading: true,
    };
  },
  mounted() {
    this.inputField = document.getElementById("movie-name");
  },
  methods: {
    fetchMovies() {
      this.loading = true;
      (this.notLoading = false),
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
          })
          .finally(() => {
            this.loading = false;
            this.notLoading = true;
          });
    },
    nominateMovie(e) {
      let length = Object.keys(this.userNominations).length;
      if (length >= 5) {
        console.log("CANNOT NOMINATE ANYMORE");
        return;
      }

      if (e.ID in this.userNominations) {
        console.log("already nominated");
        return;
      }
      this.$set(this.userNominations, e.ID, e);
      this.$emit("user-nominated", this.userNominations);
    },
    removeNomination(e) {
      this.$delete(this.userNominations, e);
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

  min-height: 800px;
  background: #222;
  margin: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.center {
  max-width: 1300px;
  width: 90%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card {
  background: #555;
}

.showResults {
  width: 100%;
  background: #111;
  margin-bottom: 20px;
}

.show-nominations {
  width: 100%;
  background: #444;
  margin-bottom: 100px;
}
.resultRow {
  display: flex;
  flex-direction: row;
  width: 100%;
  overflow: auto;
}

.input-field {
  margin: 70px 0 20px 0;
  position: relative;
  width: 450px;
  height: 180px;
  line-height: 80px;
  font-size: 25px;
  background: #555;
  width: 100%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
label {
  position: absolute;
  top: 50px;
  left: 0;
  width: 100%;
  color: #98f50c;
  transition: 0.2s all;
  cursor: text;
  font-weight: 700;
}
input {
  width: 88%;
  height: 25px;
  border: 0;
  outline: 0;
  padding: 0.5rem 15px;
  text-align: center;
  border-bottom: 2px solid #838383;
  background: rgb(49, 49, 49);
  box-shadow: none;
  color: white;
  font-size: 25px;
  border-radius: 5px;
}

input:focus,
input:valid {
  border-color: #98f50c;
}
input:focus ~ label,
input:valid ~ label {
  font-size: 22px;
  top: -0px;
  color: #98f50c;
}

/* NOMINATION BOX */
.nomBox {
  height: 50px;
  width: 100%;
  margin: 5px 0 5px 0;
  background: #555;
}

.your-head {
  color: #98f50c;
}

/* LOADING ANIMATIONS */
.loader,
.loader:before,
.loader:after {
  border-radius: 50%;
}
.loader {
  color: #98f50c;
  font-size: 11px;
  text-indent: -99999em;
  margin: 55px auto;
  position: relative;
  width: 10em;
  height: 10em;
  box-shadow: inset 0 0 0 1em;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
}
.loader:before,
.loader:after {
  position: absolute;
  content: "";
}
.loader:before {
  width: 5.2em;
  height: 10.2em;
  background: #111;
  border-radius: 10.2em 0 0 10.2em;
  top: -0.1em;
  left: -0.1em;
  -webkit-transform-origin: 5.1em 5.1em;
  transform-origin: 5.1em 5.1em;
  -webkit-animation: load2 2s infinite ease 1.5s;
  animation: load2 2s infinite ease 1.5s;
}
.loader:after {
  width: 5.2em;
  height: 10.2em;
  background: #111;
  border-radius: 0 10.2em 10.2em 0;
  top: -0.1em;
  left: 4.9em;
  -webkit-transform-origin: 0.1em 5.1em;
  transform-origin: 0.1em 5.1em;
  -webkit-animation: load2 2s infinite ease;
  animation: load2 2s infinite ease;
}
@-webkit-keyframes load2 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes load2 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>