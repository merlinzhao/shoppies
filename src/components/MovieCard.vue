<template>
  <div class="card-bg">
    <div
      :style="{ backgroundImage: `url(${posterLink})` }"
      class="posterBox"
    ></div>
    <div>
      <p>{{ title }}<br />{{ year }}</p>
    </div>
    <button
      v-on:click="nominate"
      class="nominate-button"
      type="button"
      :id="`nomBtn_${movieID}`"
    >
      {{ nominateTxt }}
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
    userNom: { type: Object, default: () => {} },
    type: { type: String, default: "nominate" },
  },
  data() {
    return { nominateTxt: "NOMINATE" };
  },
  mounted() {
    if (this.type == "nominate") {
      if (this.movieID in this.userNom) {
        this.alreadyNominated();
      }
    } else if (this.type == "remove") {
      this.setRemove();
    }
  },
  methods: {
    nominate() {
      if (this.type == "nominate") {
        if (Object.keys(this.userNom).length < 5) {
          this.alreadyNominated();
        }
        this.$emit("nominate-data", {
          Title: this.title,
          Year: this.year,
          ID: this.movieID,
          Poster: this.posterLink,
        });
      } else if (this.type == "remove") {
        this.$emit("remove-nominate", this.movieID);
      }
    },
    alreadyNominated() {
      let btnID = "nomBtn_" + this.movieID;
      let btn = document.getElementById(btnID);
      btn.disabled = true;
      btn.classList.add("nominate-button-disabled");
      document.getElementById(btnID).disabled = true;
      this.nominateTxt = "NOMINATED";
    },
    setRemove() {
      this.nominateTxt = "REMOVE";
    },
  },
};
</script>



<style scoped>
.card-bg {
  height: calc(480px - 10px);
  width: calc(250px - 10px);
  margin: 5px;
  background: #222;
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
  color: #111;
  font-weight: 700;
  background: #98f50c;
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
    #f4f524,
    #8fdc18,
    #8dbd3c,
    #98f50c,
    #8fdc18,
    #f4f524,
    #00dd22
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
  background: #72b906;
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
  background: #98f50c;
  left: 0;
  top: 0;
  border-radius: 5px;
}

.nominate-button-disabled {
  background: #555 !important;
}
.nominate-button-disabled:after {
  background: #555 !important;
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