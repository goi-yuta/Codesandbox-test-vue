<template>
  <canvas id="canvas1" width="480" height="320"></canvas>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      x: 0,
      y: 0,
      dx: 2,
      dy: -2,
      ballRadius: 10,
      intervalId: undefined,
      paddleHeight: 10,
      paddleWidth: 75,
      paddleX: 0,
      rightPressed: false,
      leftPressed: false,
    };
  },
  methods: {
    draw() {
      this.ctx.clearRect(0, 0, this.$el.width, this.$el.height);
      this.drawBall();
      this.drawPaddle();
      if (
        this.x + this.dx > this.$el.width - this.ballRadius ||
        this.x + this.dx < this.ballRadius
      ) {
        this.dx = -this.dx;
      }
      if (this.y + this.dy < this.ballRadius) {
        this.dy = -this.dy;
      } else if (this.y + this.dy > this.$el.height - this.ballRadius) {
        if (this.x > this.paddleX && this.x < this.paddleX + this.paddleWidth) {
          this.dy = -this.dy;
        } else {
          alert("GAME OVER");
          // document.location.reload();
          this.clearInterval();
        }
      }
      if (
        this.rightPressed &&
        this.paddleX < this.$el.width - this.paddleWidth
      ) {
        this.paddleX += 7;
      } else if (this.leftPressed && this.paddleX > 0) {
        this.paddleX -= 7;
      }
      this.x += this.dx;
      this.y += this.dy;
    },
    drawBall() {
      this.ctx.beginPath();
      this.ctx.arc(this.x, this.y, this.ballRadius, 0, Math.PI * 2);
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fill();
      this.ctx.closePath();
    },
    drawPaddle() {
      this.ctx.beginPath();
      this.ctx.rect(
        this.paddleX,
        this.$el.height - this.paddleHeight,
        this.paddleWidth,
        this.paddleHeight
      );
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fill();
      this.ctx.closePath();
    },
    keyDownHandler(e) {
      if (e.key === "Right" || e.key === "ArrowRight") {
        this.rightPressed = true;
      } else if (e.key === "Left" || e.key === "ArrowLeft") {
        this.leftPressed = true;
      }
    },
    keyUpHandler(e) {
      if (e.key === "Right" || e.key === "ArrowRight") {
        this.rightPressed = false;
      } else if (e.key === "Left" || e.key === "ArrowLeft") {
        this.leftPressed = false;
      }
    },
    clearInterval() {
      clearInterval(this.intervalId);
    },
  },
  mounted() {
    this.ctx = this.$el.getContext("2d");
    this.x = this.$el.width / 2;
    this.y = this.$el.height / 2;
    this.intervalId = setInterval(this.draw, 10);
    this.paddleX = (this.$el.width - this.paddleWidth) / 2;
    document.addEventListener("keydown", this.keyDownHandler, false);
    document.addEventListener("keyup", this.keyUpHandler, false);
  },
  beforeDestroy() {
    this.clearInterval();
  },
};
</script>

<style scoped>
#canvas1 {
  display: block;
  margin: 0 auto;
  background-color: #eee;
}
</style>
