<template>
  <div class="container">
    <div id="container__title">
      <h1>Pomodoro</h1>
    </div>
    <img id="container__img" src="../assets/tomate.png" alt="" />
    <div class="container__timer">
      <path id="time" />

      <div class="time-display">
        <h2>{{ timeDisplay }}</h2>
      </div>
      <button @click="handleTimer">{{ buttonText }}</button>
    </div>
  </div>
</template>

<script>
import ProgressBar from "progressbar.js";
import beep from "../assets/beep.mp3";

export default {
  name: "Home",
  data: () => {
    const pomodoroDuration = 0.1 * 60; // Cronomrtro 25 mim

    return {
      pomodoroDuration,
      restDuration: 0.1 * 60,
      currentTimeInSeconds: pomodoroDuration,
      buttonText: "Start!",
      timeP: null,
      interval: null,
      beepAudio: new Audio(beep),
    };
  },
  mounted: function () {
    this.timeP = new ProgressBar.Path(
      "#time",
      (this.pomodoroDuration + 1) * 1000
    ); //add um segundo e e converte para milis
  },
  methods: {
    handleTimer() {
      if (this.buttonText === "Start!" || this.buttonText === "Resume") {
        this.animateBar();
        this.buttonText = "Pause";
      } else if (this.buttonText === "Pause") {
        this.pauseBar();
        this.buttonText = "Resume";
      }
    },
    animateBar() {
      this.reduceTime();
      let segment = this.timeP;

      segment.animate(
        0,
        {
          duration: (this.currentTimeInSeconds + 1) * 1000,
        },
        this.onFinish
      );
    },
    pauseBar() {
      clearInterval(this.interval);
    },
    onFinish() {
      if (this.currentTimeInSeconds <= 0) {
        // Clear interval
        clearInterval(this.interval);

        // Play audio
        this.beepAudio.play();

        this.startRest();
      }
    },
    reduceTime() {
      this.interval = setInterval(() => {
        if (this.currentTimeInSeconds > 0) {
          this.currentTimeInSeconds -= 1;
        }
      }, 1000);
    },
    startRest() {
      // Set new interval
      this.reduceTime();
      setTimeout(() => {
        clearInterval(this.interval);
        this.currentTimeInSeconds = this.pomodoroDuration;
        this.buttonText = "Start!";
      });
    },
  },
  computed: {
    timeDisplay() {
      const minutes = String(parseInt(this.currentTimeInSeconds / 60));
      const seconds = String(this.currentTimeInSeconds % 60);
      const paddedMinutes = ("0" + minutes).slice(-2);
      const paddedSeconds = ("0" + seconds).slice(-2);

      return `${paddedMinutes}:${paddedSeconds}`;
    },
  },
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