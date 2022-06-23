<template>
  <div class="container">
    <div id="container__title">
      <h1>Pomodoro</h1>
    </div>
    <img id="container__img" src="../assets/tomate.png" alt="" />
    <div class="container__timer">
        <h2>25:00</h2>
      <button>Inciar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
};
</script>

<style>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#container__title {
  position: absolute;
  margin-top: 10px;
  left: 5%;
}
#container__img {
  margin-top: 70px;
}

h2 {
  font-size: 55px;
  margin-top: 70px;
  color: #fff;
  text-align: center;
}

button {
  background: #ef2b2b;
  color: #fff;
  margin-top: 90px;
  width: 150px;
  height: 55px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-size: 30px;
  text-align: center;
}
</style>