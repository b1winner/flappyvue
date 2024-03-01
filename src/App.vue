<template>
  <div id="app" @click="jump" :style="{ height: windowHeight + 'px' }">
    <div class="bird" :style="{ top: birdTop + 'px' }"></div>
    <div class="pipe" v-for="(pipe, index) in pipes" :key="index" :style="{ left: pipe.left + 'px' }">
      <div class="pipe up" :style="{ height: pipe.upHeight + 'px' }"></div>
      <div class="pipe down" :style="{ height: pipe.downHeight + 'px' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      birdTop: 220,
      windowHeight: 450,
      pipes: [],
      timer: null,
      pipeTimer: null,
    };
  },
  methods: {
    jump() {
      this.birdTop -= 80;
    },
    startMoving() {
      if (this.timer) return;
      this.timer = setInterval(() => {
        this.birdTop += 2;
        this.pipes.forEach((pipe) => {
          pipe.left -= 2;
        });
        this.checkCollision();
      }, 20);
      this.pipeTimer = setInterval(this.addPipe, 3000);
    },
    addPipe() {
      const upHeight = 100 + Math.floor(Math.random() * 150);
      const downHeight = this.windowHeight - 150 - upHeight;
      const pipe = { upHeight, downHeight, left: 500 };
      this.pipes.push(pipe);
    },
    checkCollision() {
      for (let pipe of this.pipes) {
        if (
            (this.birdTop <= pipe.upHeight || this.birdTop >= this.windowHeight - pipe.downHeight) &&
            pipe.left <= 80 && pipe.left >= 60
        ) {
          this.gameOver();
        }
      }
    },
    gameOver() {
      clearInterval(this.timer);
      clearInterval(this.pipeTimer);
      this.timer = null;
      this.pipeTimer = null;
    },
  },
  mounted() {
    this.startMoving();
  },
};
</script>

<style>
#app {
  position: relative;
  width: 400px;
  background-image: url("../flappy_assets/sprites/background-day.png");
  overflow: hidden;
}
.bird {
  width: 34px;
  height: 24px;
  background-image: url("../flappy_assets/sprites/redbird-midflap.png");
  position: absolute;
  left: 80px;
}
.pipe {
  width: 50px;
  position: absolute;
}
.pipe.up {
  position: absolute;
  top: 100%;
  background-image: url("../flappy_assets/sprites/pipe-green.png");
  transform: rotate(180deg);
}
.pipe.down {
  position: absolute;
  bottom: 00px;
  background-image: url("../flappy_assets/sprites/pipe-green.png");
}
</style>