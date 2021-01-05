<template>
  <div class="banner-bg">
    <div class="banner-img"></div>
    <h1 class="main-head">2021 Shoppies Awards</h1>
  </div>
</template>

<style scoped>
.banner-bg {
  height: 60vh;
  width: 100%;
  min-width: 500px;
  background: black;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.main-head {
  color: #98f50c;
  font-size: 65px;
  text-align: center;
  z-index: 100;
}

.banner-img {
  position: absolute;
  background-image: url("../assets/theater.png");

  height: 60vh;
  width: 100%;
  background-repeat: no-repeat;
  background-position: center center;
  align-items: center;
  background-size: cover;
  filter: saturate(0%);
}
</style>