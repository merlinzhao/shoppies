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
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      search: "",
      awaitingSearch: false,
      inputField: "",
      apikey: "8b8c26c3",
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
          console.log(response.data);
          console.log(response.data.Response);
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