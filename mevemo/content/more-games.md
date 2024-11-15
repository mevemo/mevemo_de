+++
title = "More games"
date = 2022-10-05

[taxonomies]
tags = ["games"]
+++

My short game of life in javascript attempt 

<canvas id="myCanvas" width="800" height="600"></canvas>
  <div >
  move with "Arrowkeys", shoot with "Space", enjoy.
  </div>

  <script>
    var canvas = document.getElementById("myCanvas");
    var ctx = canvas.getContext("2d");

    // Paddle and ball variables
    var ballRadius = 10;
    var x = canvas.width / 2;
    var y = canvas.height - 30;
    var dx = 2;
    var dy = -2;
    var paddleHeight = 10;
    var paddleWidth = 75;
    var paddleX = (canvas.width - paddleWidth) / 2;
    var rightPressed = false;
    var leftPressed = false;

    // Game of Life variables
    var cellSize = 10;
    var columns = Math.floor(canvas.width / cellSize);
    var rows = Math.floor(canvas.height / cellSize);
    var lifeGrid = createGrid(columns, rows);

    // Event listeners
    document.addEventListener("keydown", keyDownHandler, false);
    document.addEventListener("keyup", keyUpHandler, false);

    function keyDownHandler(e) {
      if (e.key === "Right" || e.key === "ArrowRight") {
        rightPressed = true;
      } else if (e.key === "Left" || e.key === "ArrowLeft") {
        leftPressed = true;
      } else if (e.code === "Space" || e.code === "Spacebar") {
        shootGlider(); 
      }
    }

    function keyUpHandler(e) {
      if (e.key === "Right" || e.key === "ArrowRight") {
        rightPressed = false;
      } else if (e.key === "Left" || e.key === "ArrowLeft") {
        leftPressed = false;
      }
    }

    function createGrid(columns, rows) {
      var grid = new Array(columns);
      for (var i = 0; i < columns; i++) {
        grid[i] = new Array(rows).fill(0);
      }

      // Spawn a glider at the center of the canvas
      var centerX = Math.floor(columns / 2);
      var centerY = Math.floor(rows / 2);

      grid[centerX][centerY - 1] = 1;
      grid[centerX + 1][centerY] = 1;
      grid[centerX - 1][centerY + 1] = 1;
      grid[centerX][centerY + 1] = 1;
      grid[centerX + 1][centerY + 1] = 1;

      return grid;
    }

    function drawBall() {
      ctx.beginPath();
      ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
      ctx.fillStyle = "#0095DD";
      ctx.fill();
      ctx.closePath();
    }

    function drawPaddle() {
      ctx.beginPath();
      ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
      ctx.fillStyle = "#0095DD";
      ctx.fill();
      ctx.closePath();
    }

    function drawGrid() {
      ctx.strokeStyle = "#ddd";
      for (var i = 0; i < columns; i++) {
        for (var j = 0; j < rows; j++) {
          if (lifeGrid[i][j] === 1) {
            ctx.fillStyle = "#0095DD";
            ctx.fillRect(i * cellSize, j * cellSize, cellSize, cellSize);
          }
          ctx.strokeRect(i * cellSize, j * cellSize, cellSize, cellSize);
        }
      }
    }

    function updateGrid() {
      var newGrid = createGrid(columns, rows);

      for (var i = 1; i < columns - 1; i++) {
        for (var j = 1; j < rows - 1; j++) {
          var liveNeighbors = 0;

          for (var x = -1; x <= 1; x++) {
            for (var y = -1; y <= 1; y++) {
              liveNeighbors += lifeGrid[i + x][j + y];
            }
          }

          liveNeighbors -= lifeGrid[i][j];

          if (lifeGrid[i][j] === 1 && (liveNeighbors < 2 || liveNeighbors > 3)) {
            newGrid[i][j] = 0;
          } else if (lifeGrid[i][j] === 0 && liveNeighbors === 3) {
            newGrid[i][j] = 1;
          } else {
            newGrid[i][j] = lifeGrid[i][j];
          }
        }
      }

      lifeGrid = newGrid;
    }

    function shootLife() {
      var columnIndex = Math.floor(paddleX / cellSize);
      // Check if there is enough space to spawn the glider
      if (columnIndex > 0 && columnIndex < columns - 2) {
        lifeGrid[columnIndex][rows - 6] = 1;
        lifeGrid[columnIndex + 1][rows - 5] = 1;
        lifeGrid[columnIndex + 2][rows - 5] = 1;
        lifeGrid[columnIndex + 2][rows - 6] = 1;
        lifeGrid[columnIndex + 1][rows - 7] = 1;
        }}


    function shootGlider() {
      var columnIndex = Math.floor(paddleX / cellSize);

      // Check if there is enough space to spawn the glider
      if (columnIndex > 0 && columnIndex < columns - 2) {
        lifeGrid[columnIndex][rows - 7] = 1;
        lifeGrid[columnIndex - 1][rows - 7] = 1;
        lifeGrid[columnIndex - 1][rows - 6] = 1;
        lifeGrid[columnIndex - 1][rows - 5] = 1;
        lifeGrid[columnIndex + 1][rows - 6] = 1;
      }
    }

    function spawnGlider(x, y) {
      if (x >= 0 && x < columns - 2 && y >= 0 && y < rows - 2) {
        lifeGrid[x][y - 1] = 1;
        lifeGrid[x + 1][y] = 1;
        lifeGrid[x - 1][y + 1] = 1;
        lifeGrid[x][y + 1] = 1;
        lifeGrid[x + 1][y + 1] = 1;
      }
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawBall();
      drawPaddle();
      drawGrid();

      if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
        spawnGlider(8,8);
        dx = -dx;
      }
      if (y + dy > canvas.height - ballRadius || y + dy < ballRadius) {
        spawnGlider(8,8);
        dy = -dy;
      }

      if (rightPressed) {
        paddleX += 7;
        if (paddleX + paddleWidth > canvas.width) {
          paddleX = canvas.width - paddleWidth;
        }
      } else if (leftPressed) {
        paddleX -= 7;
        if (paddleX < 0) {
          paddleX = 0;
        }
      }

      if (y + dy >= canvas.height - ballRadius - paddleHeight) {
        if (x >= paddleX && x <= paddleX + paddleWidth) {
        spawnGlider(8,8);
          shootLife();
          dy = -dy;
        }
      }
      x += dx;
      y += dy;
      updateGrid();
    }

    function setStage(){
      for (var i; i<5; i++){
        var z = 3 * i;
        lifeGrid[z][3] = 1;
        lifeGrid[z][4] = 1;
        lifeGrid[z+1][3] = 1;
        lifeGrid[z+1][4] = 1;
      }
    }
    setStage();

    setInterval(draw, 10);
  </script>
