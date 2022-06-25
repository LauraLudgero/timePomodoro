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
    <button v-if="no" @click="button1">{{ buttonText }}</button><p v-if="no">Sem Descaso</p>
    <button v-if="yes" @click="button2">{{ buttonText }}</button><p v-if="yes">Com Descaso</p>  
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
      currentSegment: 1,
      buttonText: "Inciar",
      timeP: null,
      interval: null,
      beepAudio: new Audio(beep),
      resting: false,
            no: true,
      yes: true,
      test: null,
    };
  },
  mounted: function () {
    this.timeP = new ProgressBar.Path(
      "#time",
      (this.pomodoroDuration + 1) * 1000
    ); //add um segundo e e converte para milis
  },
  methods: {
        button1() {
      this.yes = false;
      this.test = 1;

      this.handleTimer();
    },
    button2() {
      this.no = false;
      this.test = 2;

      this.handleTimer();
    },
    handleTimer() {
      if (this.buttonText === "Inciar" || this.buttonText === "Continuar") {
        this.animateBar();
        this.buttonText = "Pausar";
      } else if (this.buttonText === "Pausar") {
        this.pauseBar();
        this.buttonText = "Continuar";
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

        
        if (this.test == 2) {
          // Immediately disable button and set state
          this.resting = true;
          this.buttonText = "Rest";

          setTimeout(() => {
            // Change time to reflect rest duration
            this.currentTimeInSeconds = this.restDuration;

            // Start rest after beep ends
            this.startRest();
          }, 4200);
        } else {
          this.start();
        }
      }
    },
    reduceTime() {
      this.interval = setInterval(() => {
        if (this.currentTimeInSeconds > 0) {
          this.currentTimeInSeconds -= 1;
        }
      }, 1000);
    },
        start() {
      // Set new interval
      this.reduceTime();
      setTimeout(() => {
        clearInterval(this.interval);
        this.beepAudio.play();
        this.currentTimeInSeconds = this.pomodoroDuration;
        this.buttonText = "Inciar";
        this.resting = false;
        this.yes = true;
      }, );
    },
    startRest() {
      // Set new interval
      this.reduceTime();
      setTimeout(() => {
        clearInterval(this.interval);
        this.beepAudio.play();
        this.currentTimeInSeconds = this.pomodoroDuration;
        this.buttonText = "Inciar";
        this.resting = false;
        this.no = true;
      },
       this.restDuration * 1000
       );
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

p{
  font-size: 30px;
  line-height: 48px;
  text-align: center;
  color: #fff;
}

button {
  background: #ef2b2b;
  color: #fff;
  margin-top: 20px;
  width: 150px;
  height: 55px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-size: 30px;
  text-align: center;
  flex: 10px;
  display: inline-block;
}
</style>