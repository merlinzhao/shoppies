<template>
  <div class="card-bg">
    <div
      :style="{ backgroundImage: `url(${posterLink})` }"
      class="posterBox"
    ></div>
    <div>
      <p>{{ title }}<br />{{ year }}</p>
    </div>
    <button v-on:click="nominate" class="nominate-button" type="button">
      NOMINATE
    </button>
  </div>
</template>



<script>
export default {
  props: {
    title: { type: String, default: "Title not found" },
    year: { type: String, default: "0000" },
    posterLink: { type: String, default: "NONE" },
    movieID: { type: String, default: "NONE" },
  },
  data() {
    return {};
  },
  methods: {
    nominate() {
      this.$emit("nominate-data", {
        Title: this.title,
        Year: this.year,
        ID: this.movieID,
      });
    },
  },
};
</script>



<style scoped>
.card-bg {
  height: calc(480px - 10px);
  width: calc(250px - 10px);
  margin: 5px;
  background: #171717;
  color: #aaa;
  font-weight: 700;
}

.posterBox {
  width: 100%;
  height: 320px;
  background-repeat: no-repeat;
  background-position: center center;
  align-items: center;
  background-size: cover;
}

/* NOMINATE BUTTON HOVER ANIMATIONS */

.nominate-button {
  width: 80%;
  height: 40px;
  border: none;
  outline: none;
  color: #fff;
  background: rgb(255, 187, 0);
  cursor: pointer;
  position: relative;
  z-index: 0;
  transition: all 0.3s ease-out;
  border-radius: 5px;
}

.nominate-button:before {
  content: "";
  background: linear-gradient(
    45deg,
    #ff0000,
    #ff7300,
    #fffb00,
    #48ff00,
    #00ffd5,
    #002bff,
    #7a00ff,
    #ff00c8,
    #ff0000
  );
  position: absolute;
  top: -2px;
  left: -2px;
  background-size: 400%;
  z-index: -1;
  filter: blur(5px);
  width: calc(100% + 4px);
  height: calc(100% + 4px);
  animation: glowing 20s linear infinite;
  opacity: 0;
  transition: opacity 0.3s ease-in-out;

  border-radius: 5px;
}

.nominate-button:active {
  color: #000;
}

.nominate-button:active:after {
  background: rgb(255, 128, 69);
}

.nominate-button:hover:before {
  opacity: 1;
}

.nominate-button:after {
  z-index: -1;
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgb(255, 187, 0);
  left: 0;
  top: 0;
  border-radius: 5px;
}

@keyframes glowing {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 400% 0;
  }
  100% {
    background-position: 0 0;
  }
}
</style>