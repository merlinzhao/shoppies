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
        <label for="movie-name">Search Films </label>
      </div>
      <div class="showResults">
        <div class="row resultRow">
          <div class="loader-box" v-if="loading">
            <div class="loader"></div>
          </div>
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
          <div v-if="showPrompt" class="show-prompt">
            <h2 class="your-head">{{ promptHead }}</h2>
            <p class="your-text">{{ promptText }}</p>
          </div>
        </div>
      </div>
      <div class="show-nominations">
        <h1 class="your-head">Your 2021 Nominations</h1>
        <p class="your-text">
          {{ yourText }}
        </p>
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
      showPrompt: true,
      promptHead: "Start Nominating",
      promptText:
        "Search for a movie that you want to see win a Shoppie. You may nominate up to 5 films.",
      yourText:
        "Nominate up to 5 films that you think deserve a 2021 Shoppie. Your nominations below will be submitted automatically on 11:59pm Janurary 17, 2021.",
    };
  },
  mounted() {
    this.inputField = document.getElementById("movie-name");
  },
  methods: {
    fetchMovies() {
      this.loading = true;
      this.showPrompt = false;
      this.promptHead = "Start Nominating";
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
            if (valid == "True") {
              this.movieResults = response.data.Search;
            } else {
              if (response.data.Error == "Too many results.") {
                this.promptHead = "Alter your search.";
                this.promptText =
                  "Please be more specific with your search. We were unable to find the films you are looking for.";
              } else {
                this.promptHead = "Start Nominating";
                this.promptText =
                  "Search for a movie that you want to see win a Shoppie. You may nominate up to 5 films.";
              }
              this.showPrompt = true;
              this.movieResults = {};
            }
          })
          .catch(() => {
            console.log("ERROR FETCHING");
          })
          .finally(() => {
            this.loading = false;
            this.notLoading = true;

            if (!this.movieResults) {
              this.showPrompt = true;
            }
          });
    },
    nominateMovie(e) {
      let length = Object.keys(this.userNominations).length;
      if (length >= 5) {
        return;
      }
      if (e.ID in this.userNominations) {
        return;
      }
      if (length == 4) {
        this.yourText =
          "You have nominated a maximum of 5 films for the 2021 Shoppie Awards. If you wish to change your nominations, remove one from your list and nominate another film. You may remove nominations up until 11:59pm on Janurary 17, 2021.";
      }
      this.$set(this.userNominations, e.ID, e);
      this.$emit("user-nominated", this.userNominations);
    },
    removeNomination(e) {
      this.$delete(this.userNominations, e);
      this.showPrompt = true;
      this.promptHead = "Start Nominating";
      this.yourText =
        "Nominate up to 5 films that you think deserve a 2021 Shoppie. Your nominations below will be submitted automatically at 11:59pm on Janurary 17, 2021.";
      this.promptText =
        "Search for a movie that you want to see win a Shoppie. You may nominate up to 5 films.";
      this.movieResults = {};
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
  width: 100%;

  min-height: 800px;
  background: #222;
  margin: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.center {
  min-width: 500px;
  max-width: 1300px;
  width: 90%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card {
  background: #444;
}

.showResults {
  width: 100%;
  background: #444;
  margin-bottom: 20px;
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
  background: #444;
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

/* PROMPT BOX ====================*/
.show-prompt {
  height: 500px;
  width: 100%;
  background: #444;
}

/* NOMINATION BOX ====================*/

.show-nominations {
  width: 100%;
  background: #444;
  margin-bottom: 100px;
}
.your-head {
  color: #98f50c;
  margin: 20px 0 10px 0;
}
.your-text {
  color: rgb(216, 255, 209);
  margin: 0px 30px 20px 30px;
}

/* LOADING ANIMATIONS ================*/

.loader-box {
  width: 100%;
  height: 500px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
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
  position: absolute;

  left: 45%;
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
  background: #444;
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
  background: #444;
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