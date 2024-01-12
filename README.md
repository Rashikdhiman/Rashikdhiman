<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      overflow: hidden;
    }

    canvas {
      display: block;
    }
  </style>
  <title>Simple Shooting Game</title>
</head>
<body>
  <canvas id="gameCanvas"></canvas>
  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const player = {
      x: canvas.width / 2,
      y: canvas.height - 30,
      width: 30,
      height: 30,
      color: 'blue',
      speed: 5,
    };

    const bullets = [];

    const targets = [];

    function drawPlayer() {
      ctx.fillStyle = player.color;
      ctx.fillRect(player.x - player.width / 2, player.y - player.height / 2, player.width, player.height);
    }

    function drawBullets() {
      ctx.fillStyle = 'red';
      bullets.forEach(bullet => {
        ctx.fillRect(bullet.x - 5, bullet.y - 15, 10, 15);
      });
    }

    function drawTargets() {
      ctx.fillStyle = 'green';
      targets.forEach(target => {
        ctx.fillRect(target.x - 15, target.y - 15, 30, 30);
      });
    }

    function update() {
      drawPlayer();
      drawBullets();
      drawTargets();

      // Update bullet positions
      bullets.forEach(bullet => {
        bullet.y -= 5;
      });

      // Check for collisions
      bullets.forEach((bullet, bulletIndex) => {
        targets.forEach((target, targetIndex) => {
          if (
            bullet.x > target.x - 15 &&
            bullet.x < target.x + 15 &&
            bullet.y > target.y - 15 &&
            bullet.y < target.y + 15
          ) {
            // Collision detected, remove bullet and target
            bullets.splice(bulletIndex, 1);
            targets.splice(targetIndex, 1);
          }
        });
      });
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      update();
      requestAnimationFrame(gameLoop);
    }

    function shoot() {
      bullets.push({ x: player.x, y: player.y - player.height / 2 });
    }

    function handleKeyPress(event) {
      switch (event.key) {
        case 'ArrowLeft':
          player.x -= player.speed;
          break;
        case 'ArrowRight':
          player.x += player.speed;
          break;
        case ' ':
          shoot();
          break;
      }
    }

    window.addEventListener('keydown', handleKeyPress);

    // Initial targets
    for (let i = 0; i < 5; i++) {
      targets.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height / 2,
      });
    }

    gameLoop();
  </script>
</body>
</html>

