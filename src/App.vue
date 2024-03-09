<!--npm run serve-->

<template>
  <div id="app" @click="jump" :style="{ height: windowHeight + 'px' }">
    <div class="bird" :style="{ top: birdy + 'px' }"></div>
    <div class="pipe" v-for="(pipe, index) in pipes" :key="index" :style="{ left: pipe.left + 'px' }">
      <div class="pipe top" :style="{ height: pipe.topHeight + 'px' }"></div>
      <div class="space" :style="{ height: '150px' }"></div>
      <div class="pipe bottom" :style="{ height: pipe.bottomHeight + 'px' }"></div>
    </div>
  </div>
<!--  <div id="text">
    <h1>Flappy Vue!</h1>

  </div>-->
</template>

<script>
export default {
  data() {
    return {
      birdy: 220,
      windowHeight: 450,
      pipes: [],
      timer: null,
      pipeTimer: null,
      score: 0,
    };
  },
  methods: {
    jump() {
      this.birdy -= 90;
    },
    checkLose() {
      for (let pipe of this.pipes) {
        if (
            (this.birdy <= pipe.topHeight || this.birdy >= this.windowHeight - pipe.bottomHeight) &&
            pipe.left <= 80 && pipe.left >= 60
        ) {
          this.stop();
        }
        else if (pipe.left === 60){ //increase score
          this.score+=1;
          console.log(this.score);
        }
      }
    },
    run() {
      if (this.timer) return;
      this.timer = setInterval(() => {
        this.birdy += 2;
        this.pipes.forEach((pipe) => {
          pipe.left -= 2;
        });
        this.checkLose();
      }, 20);
      this.pipeTimer = setInterval(this.newPipe, 2500); /*Spawn Rate*/
    },
    randomtopHeight() {
      const howRandom = 222;
      return Math.floor(Math.random() * howRandom);
    },
    newPipe() {
      const gapSize = 150;
      const topHeight = this.randomtopHeight();
      const bottomHeight = this.windowHeight - gapSize - topHeight;
      const pipe = { topHeight, bottomHeight, left: 450 }; /*start at 450 px when screen width is 400*/
      this.pipes.push(pipe);
    },
    stop() {
      /*Stop run*/
      clearInterval(this.timer);
      clearInterval(this.pipeTimer);
    },
  },
  mounted() { /*starter*/
    this.run();
  },
};
</script>

<style>
#text {
  background: azure;
}
#app {
  position: relative;
  width: 400px;
  background-image: url("../flappy_assets/sprites/background-day.png");
  overflow: hidden;  /*hide offscreen stuff*/
}
.bird {
  width: 34px;
  height: 24px;
  background-image: url("../flappy_assets/sprites/redbird-midflap.png");
  position: absolute;
  left: 80px; /*Shift Right from left side*/
}
.pipe {
  width: 50px;
  position: absolute;
}
.pipe.top {
  position: relative;
  background-image: url("../flappy_assets/sprites/pipe-green.png");
  transform: rotate(180deg);
}
.space {
  position: relative;
  color: aliceblue;
}
.pipe.bottom {
  position: absolute;
  background-image: url("../flappy_assets/sprites/pipe-green.png");
}
</style>