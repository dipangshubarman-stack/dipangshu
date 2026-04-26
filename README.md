<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Love Clock ❤️</title>

<style>
body {
  margin: 0;
  overflow: hidden;
  background: black;
  font-family: 'Poppins', sans-serif;
}

/* Canvas for hearts */
canvas {
  position: fixed;
  top: 0;
  left: 0;
}

/* Clock UI */
.clock {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  z-index: 10;
}

.time {
  font-size: 70px;
  font-weight: bold;
}

.date {
  font-size: 20px;
  color: #d78811;
}
</style>
</head>

<body>

<canvas id="screen"></canvas>

<div class="clock">
  <div class="time" id="time">00:00:00</div>
  <div class="date" id="date">Loading...</div>
</div>

<script>
// ================= CLOCK =================
function updateClock() {
  const now = new Date();

  let h = now.getHours();
  let m = now.getMinutes();
  let s = now.getSeconds();

  h = h < 10 ? "0" + h : h;
  m = m < 10 ? "0" + m : m;
  s = s < 10 ? "0" + s : s;

  document.getElementById("time").innerText = `${h}:${m}:${s}`;

  document.getElementById("date").innerText =
    now.toLocaleDateString(undefined, {
      weekday: 'long',
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    });
}

setInterval(updateClock, 1000);
updateClock();


// ================= HEARTS TRAIL =================
const canvas = document.getElementById("screen");
const ctx = canvas.getContext("2d");

let width, height;

function resize() {
  width = canvas.width = window.innerWidth;
  height = canvas.height = window.innerHeight;
}
window.addEventListener("resize", resize);
resize();

let pointer = { x: width / 2, y: height / 2 };

window.addEventListener("pointermove", (e) => {
  pointer.x = e.clientX;
  pointer.y = e.clientY;
  hearts.push(new Heart(pointer.x, pointer.y));
});

let hearts = [];

class Heart {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = Math.random() * 10 + 10;
    this.alpha = 1;
    this.speedY = Math.random() * 1 + 0.5;
    this.speedX = (Math.random() - 0.5) * 1;
  }

  draw() {
    ctx.save();
    ctx.globalAlpha = this.alpha;
    ctx.fillStyle = "red";

    let s = this.size;
    ctx.beginPath();
    ctx.moveTo(this.x, this.y);
    ctx.bezierCurveTo(this.x - s, this.y - s, this.x - s * 2, this.y + s / 2, this.x, this.y + s * 2);
    ctx.bezierCurveTo(this.x + s * 2, this.y + s / 2, this.x + s, this.y - s, this.x, this.y);
    ctx.fill();

    ctx.restore();
  }

  update() {
    this.y -= this.speedY;
    this.x += this.speedX;
    this.alpha -= 0.02;
  }
}

function animate() {
  ctx.clearRect(0, 0, width, height);

  for (let i = hearts.length - 1; i >= 0; i--) {
    hearts[i].update();
    hearts[i].draw();

    if (hearts[i].alpha <= 0) {
      hearts.splice(i, 1);
    }
  }

  requestAnimationFrame(animate);
}

animate();
</script>

</body>
</html>
