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
      intervalId: undefined
    };
  },
  methods: {
    draw() {
      this.ctx.clearRect(0, 0, this.$el.width, this.$el.height);
      this.ctx.beginPath();
      this.ctx.arc(this.x, this.y, this.ballRadius, 0, Math.PI * 2);
      this.ctx.fillStyle = "#0095DD";
      this.ctx.fill();
      this.ctx.closePath();
      if (
        this.x + this.dx > this.$el.width - this.ballRadius ||
        this.x + this.dx < this.ballRadius
      ) {
        this.dx = -this.dx;
      }
      if (
        this.y + this.dy > this.$el.height - this.ballRadius ||
        this.y + this.dy < this.ballRadius
      ) {
        this.dy = -this.dy;
      }
      this.x += this.dx;
      this.y += this.dy;
    }
  },
  mounted() {
    this.ctx = this.$el.getContext("2d");
    this.x = this.$el.width / 2;
    this.y = this.$el.height / 2;
    this.intervalId = setInterval(this.draw, 10);
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  }
};
</script>

<style scoped>
#canvas1 {
  display: block;
  margin: 0 auto;
  background-color: #eee;
}
</style>
