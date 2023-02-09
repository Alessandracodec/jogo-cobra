# jogo-cobra
jogo cobrinha (snake)


<!DOCTYPE html>
<html>
<head>
  <style>
    #game-board {
      width: 500px;
      height: 500px;
      border: 1px solid black;
      margin: 0 auto;
      position: relative;
    }

    .snake-unit {
      width: 10px;
      height: 10px;
      position: absolute;
      background-color: green;
    }

    .food-unit {
      width: 10px;
      height: 10px;
      position: absolute;
      background-color: red;
    }
  </style>
</head>
<body>
  <div id="game-board"></div>

  <script>
    const gameBoard = document.getElementById("game-board");
    const snake = [];
    let food;

    // Inicializa a cobra com três unidades
    for (let i = 0; i < 3; i++) {
      snake.push({ x: i, y: 0 });
    }

    // Desenha a cobra no tabuleiro de jogo
    function drawSnake() {
      snake.forEach(function(snakeUnit) {
        const unitElement = document.createElement("div");
        unitElement.style.left = `${snakeUnit.x * 10}px`;
        unitElement.style.top = `${snakeUnit.y * 10}px`;
        unitElement.classList.add("snake-unit");
        gameBoard.appendChild(unitElement);
      });
    }

    // Gera comida aleatória no tabuleiro de jogo
    function generateFood() {
      food = { x: Math.floor(Math.random() * 50), y: Math.floor(Math.random() * 50) };

      const foodElement = document.createElement("div");
      foodElement.style.left = `${food.x * 10}px`;
      foodElement.style.top = `${food.y * 10}px`;
      foodElement.classList.add("food-unit");
      gameBoard.appendChild(foodElement);
    }

    drawSnake();
    generateFood();

    // Adiciona lógica de movimento à cobra
    let direction = "right";
    document.onkeydown = function handleKeyDown(e) {
      const key = e.keyCode;
      if (key === 37 && direction !== "right") {
        direction = "left";
      } else if (key === 38 && direction !== "down") {
        direction = "up";
      } else if (key === 39 && direction !== "left") {
        direction = "right";
      } else if (key === 40 && direction !== "up") {
        direction = "down";
      }
    };

    function update() {
      let snakeHeadX = snake[0].x;
      let snakeHeadY = snake[0].y;

      if (direction === "right") {
        snakeHeadX++;
      } else if (direction === "left") {
           snakeHeadX--;
  } else if (direction === "up") {
    snakeHeadY--;
  } else if (direction === "down") {
    snakeHeadY++;
  }

  // Verifica se a cobra bateu na parede
  if (snakeHeadX < 0 || snakeHeadX >= 50 || snakeHeadY < 0 || snakeHeadY >= 50) {
    alert("Game Over");
    clearInterval(gameLoop);
    return;
  }

  // Verifica se a cobra comeu a comida
  if (snakeHeadX === food.x && snakeHeadY === food.y) {
    const tail = { x: snakeHeadX, y: snakeHeadY };
    generateFood();
  } else {
    const tail = snake.pop();
    tail.x = snakeHeadX;
    tail.y = snakeHeadY;
  }

  // Verifica se a cobra colidiu com o próprio corpo
  for (let i = 0; i < snake.length; i++) {
    if (snake[i].x === snakeHeadX && snake[i].y === snakeHeadY) {
      alert("Game Over");
      clearInterval(gameLoop);
      return;
    }
  }

  snake.unshift(tail);

  gameBoard.innerHTML = "";
  drawSnake();
}

let gameLoop = setInterval(update, 100);
  </script>
</body>
</html>

