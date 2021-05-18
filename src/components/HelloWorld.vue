<template>
  <canvas id="canvas1" width="480" height="320"></canvas>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    play: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      score: 0,
      lives: 3,
      x: 0,
      y: 0,
      dx: 2,
      dy: -2,
      ballRadius: 10,
      interval: undefined,
      paddleHeight: 10,
      paddleWidth: 75,
      paddleX: 0,
      rightPressed: false,
      leftPressed: false,
      bricks: [],
      brickRowCount: 1,
      brickColumnCount: 5,
      brickWidth: 75,
      brickHeight: 20,
      brickPadding: 10,
      brickOffsetTop: 30,
      brickOffsetLeft: 30,
    };
  },
  watch: {
    play: function (newVal, oldVal) {
      if (newVal) {
        this.init();
        this.draw();
      }
      if (oldVal) {
        cancelAnimationFrame(this.interval);
      }
    },
  },
  methods: {
    init() {
      this.score = 0;
      this.lives = 3;
      this.bricks = [];
      this.x = this.$el.width / 2;
      this.y = this.$el.height / 2;
      this.dx = 2;
      this.dy = -2;
      this.paddleX = (this.$el.width - this.paddleWidth) / 2;
      for (let c = 0; c < this.brickColumnCount; c++) {
        this.bricks[c] = [];
        for (let r = 0; r < this.brickRowCount; r++) {
          this.bricks[c][r] = { x: 0, y: 0, status: 1 };
        }
      }
    },
    draw() {
      this.ctx.clearRect(0, 0, this.$el.width, this.$el.height);
      this.drawBall();
      this.drawPaddle();
      this.drawBricks();
      this.drawScore();
      this.drawLives();
      this.collisionDetection();
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
          this.lives--;
          if (this.lives <= 0) {
            this.$emit("reset");
          } else {
            this.x = this.$el.width / 2;
            this.y = this.$el.height - 30;
            this.dx = 3;
            this.dy = -3;
            this.paddleX = (this.$el.width - this.paddleWidth) / 2;
          }
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
      this.interval = requestAnimationFrame(this.draw);
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
    drawBricks() {
      for (let c = 0; c < this.brickColumnCount; c++) {
        for (let r = 0; r < this.brickRowCount; r++) {
          if (this.bricks[c][r].status === 1) {
            const brickX =
              c * (this.brickWidth + this.brickPadding) + this.brickOffsetLeft;
            const brickY =
              r * (this.brickHeight + this.brickPadding) + this.brickOffsetTop;
            this.bricks[c][r].x = brickX;
            this.bricks[c][r].y = brickY;
            this.ctx.beginPath();
            this.ctx.rect(brickX, brickY, this.brickWidth, this.brickHeight);
            this.ctx.fillStyle = "#0095DD";
            this.ctx.fill();
            this.ctx.closePath();
          }
        }
      }
    },
    collisionDetection() {
      for (let c = 0; c < this.brickColumnCount; c++) {
        for (let r = 0; r < this.brickRowCount; r++) {
          if (this.bricks[c][r].status === 1) {
            if (
              this.x > this.bricks[c][r].x &&
              this.x < this.bricks[c][r].x + this.brickWidth &&
              this.y > this.bricks[c][r].y &&
              this.y < this.bricks[c][r].y + this.brickHeight
            ) {
              this.dy = -this.dy;
              this.bricks[c][r].status = 0;
              this.score++;
              if (this.score === this.brickRowCount * this.brickColumnCount) {
                alert("YOU WIN, CONGRATULATIONS!");
                document.location.reload();
              }
            }
          }
        }
      }
    },
    drawScore() {
      this.ctx.font = "16px Arial";
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fillText("Score: " + this.score, 8, 20);
    },
    drawLives() {
      this.ctx.font = "16px Arial";
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fillText("Lives: " + this.lives, this.$el.width - 65, 20);
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
    mouseMoveHandler(e) {
      const relativeX = e.clientX - this.$el.offsetLeft;
      if (relativeX > 0 && relativeX < this.$el.width) {
        this.paddleX = relativeX - this.paddleWidth / 2;
      }
    },
  },
  mounted() {
    this.ctx = this.$el.getContext("2d");
    document.addEventListener("keydown", this.keyDownHandler, false);
    document.addEventListener("keyup", this.keyUpHandler, false);
    document.addEventListener("mousemove", this.mouseMoveHandler, false);
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
