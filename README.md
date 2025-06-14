<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>贪吃蛇游戏</title>
  <style>
    body {
      background: #222;
      color: #fff;
      text-align: center;
      font-family: Arial;
    }
    canvas {
      background: #111;
      display: block;
      margin: 20px auto;
      border: 2px solid #fff;
    }
    #scoreboard {
      font-size: 20px;
    }
  </style>
</head>
<body>

<h1>贪吃蛇游戏</h1>
<div id="scoreboard">得分：0 | 时间：0 秒</div>
<canvas id="gameCanvas" width="400" height="400"></canvas>

<script>
  const canvas = document.getElementById("gameCanvas");
  const ctx = canvas.getContext("2d");

  const box = 20;
  let score = 0;
  let time = 0;
  let gameInterval, timerInterval;

  let snake = [{ x: 9 * box, y: 10 * box }];
  let food = {
    x: Math.floor(Math.random() * 19 + 1) * box,
    y: Math.floor(Math.random() * 19 + 1) * box
  };
  let direction = "";

  document.addEventListener("keydown", changeDirection);

  function changeDirection(event) {
    if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
    if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
    if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
    if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
  }

  function draw() {
    ctx.fillStyle = "#111";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    for (let i = 0; i < snake.length; i++) {
      ctx.fillStyle = i === 0 ? "lime" : "green";
      ctx.fillRect(snake[i].x, snake[i].y, box, box);
    }

    ctx.fillStyle = "red";
    ctx.fillRect(food.x, food.y, box, box);

    let snakeX = snake[0].x;
    let snakeY = snake[0].y;

    if (direction === "LEFT") snakeX -= box;
    if (direction === "UP") snakeY -= box;
    if (direction === "RIGHT") snakeX += box;
    if (direction === "DOWN") snakeY += box;

    if (snakeX === food.x && snakeY === food.y) {
      score++;
      food = {
        x: Math.floor(Math.random() * 19 + 1) * box,
        y: Math.floor(Math.random() * 19 + 1) * box
      };
    } else {
      snake.pop();
    }

    let newHead = { x: snakeX, y: snakeY };

    // 碰撞检测
    if (
      snakeX < 0 || snakeX >= canvas.width ||
      snakeY < 0 || snakeY >= canvas.height ||
      collision(newHead, snake)
    ) {
      clearInterval(gameInterval);
      clearInterval(timerInterval);
      alert("游戏结束！得分：" + score + "，用时：" + time + "秒");
    }

    snake.unshift(newHead);
    document.getElementById("scoreboard").innerText = `得分：${score} | 时间：${time} 秒`;
  }

  function collision(head, array) {
    for (let i = 0; i < array.length; i++) {
      if (head.x === array[i].x && head.y === array[i].y) {
        return true;
      }
    }
    return false;
  }

  // 启动游戏
  gameInterval = setInterval(draw, 150);
  timerInterval = setInterval(() => time++, 1000);
</script>

</body>
</html>
