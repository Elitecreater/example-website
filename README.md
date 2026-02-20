<!DOCTYPE html>
<html>
<head>
    <title>Pong Game</title>
    <style>
        body {
            background: #000;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        canvas {
            border: 2px solid #fff;
        }
    </style>
</head>
<body>
<canvas id="gameCanvas" width="800" height="500"></canvas>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

// Ball
let ballX = canvas.width / 2;
let ballY = canvas.height / 2;
let ballSpeedX = 5;
let ballSpeedY = 5;

// Paddles
const paddleHeight = 100;
const paddleWidth = 10;

let leftPaddleY = canvas.height / 2 - paddleHeight / 2;
let rightPaddleY = canvas.height / 2 - paddleHeight / 2;

// Player controls
let upPressed = false;
let downPressed = false;

document.addEventListener("keydown", (e) => {
    if (e.key === "ArrowUp") upPressed = true;
    if (e.key === "ArrowDown") downPressed = true;
});
document.addEventListener("keyup", (e) => {
    if (e.key === "ArrowUp") upPressed = false;
    if (e.key === "ArrowDown") downPressed = false;
});

function drawRect(x, y, w, h, color) {
    ctx.fillStyle = color;
    ctx.fillRect(x, y, w, h);
}

function drawBall() {
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.arc(ballX, ballY, 10, 0, Math.PI * 2);
    ctx.fill();
}

function moveEverything() {
    // Ball movement
    ballX += ballSpeedX;
    ballY += ballSpeedY;

    // Ball bounce top/bottom
    if (ballY < 0 || ballY > canvas.height) {
        ballSpeedY = -ballSpeedY;
    }

    // Ball bounce left paddle
    if (ballX < 20 && ballY > leftPaddleY && ballY < leftPaddleY + paddleHeight) {
        ballSpeedX = -ballSpeedX;
    }

    // Ball bounce right paddle
    if (ballX > canvas.width - 20 && ballY > rightPaddleY && ballY < rightPaddleY + paddleHeight) {
        ballSpeedX = -ballSpeedX;
    }

    // Reset ball if missed
    if (ballX < 0 || ballX > canvas.width) {
        ballX = canvas.width / 2;
        ballY = canvas.height / 2;
    }

    // Player paddle movement
    if (upPressed && rightPaddleY > 0) rightPaddleY -= 7;
    if (downPressed && rightPaddleY < canvas.height - paddleHeight) rightPaddleY += 7;

    // Simple AI for left paddle
    if (leftPaddleY + paddleHeight / 2 < ballY) leftPaddleY += 4;
    if (leftPaddleY + paddleHeight / 2 > ballY) leftPaddleY -= 4;
}

function drawEverything() {
    drawRect(0, 0, canvas.width, canvas.height, "black");
    drawBall();
    drawRect(10, leftPaddleY, paddleWidth, paddleHeight, "white");
    drawRect(canvas.width - 20, rightPaddleY, paddleWidth, paddleHeight, "white");
}

function gameLoop() {
    moveEverything();
    drawEverything();
    requestAnimationFrame(gameLoop);
}

gameLoop();
</script>
</body>
</html>
