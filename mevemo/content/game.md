+++
title = "Ball World"
date = 2019-01-03

[taxonomies]
tags = ["javascript","games", "mevemo"]
+++

<canvas id="myCanvas" width="900" height="600"></canvas>
<table>
<tr>
    <td></td>
    <td></td>
    <td></td>
 <td colspan=3>
  <table class="logTable">
    <tr>
      <th>__________</th>
      <th>X Position</th>
      <th>Y Position</th>
      <th>delta X</th>
      <th>delta Y</th>
    </tr>
    <tr>
      <td>Player</td>
      <td>
        <div id="logP"> </div> 
      </td>
      <td>
        <div id="logP_Y"> </div> 
      </td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Ball</td>
      <td>
        <div id="logB"> </div>
      </td>
      <td>
        <div id="logB_Y"> </div>
      </td>
      <td>
        <div id="logB_dx"> </div>
      </td>
      <td>
        <div id="logB_dy"> </div>
      </td>
    </tr>
    <tr>
      <td>Touch</td>
      <td>
        <div id="logT"> </div>
      </td>
      <td>
        <div id="logT_Y"> </div>
      </td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Bullets</td>
      <td>
        <div id="logBullets"> </div>
      </td>
      <td>
        <div id="logT_Y"> </div>
      </td>
      <td></td>
      <td></td>
    </tr>
  </table>

 </td>
</tr>
<tr>
  <td></td>
  <td><div id="up-touch">UP</div></td>
  <td></td>
  <td></td>
  <td></td>
  <td><div class="roundButton"  id="a-touch">A</div></td>
</tr>
<tr>
  <td><div id="left-touch">LEFT</div></td>
  <td><div id="center-touch"></td>
  <td><div id="right-touch">RIGHT</div></td>
  <td><div id="center-touch"></td>
  <td><div id="b-touch" class="roundButton">B</div></td>
  <td></td>
</tr>
<tr>
  <td></td>
  <td><div id="down-touch">DOWN</div></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
</table>

<script>
// JavaScript code goes here
let canvas = document.getElementById("myCanvas");
let ctx = canvas.getContext("2d");
let bgColor = "#dde4bb" ; // "#0095DD";
let entropy = 1;
let zz = 0;
var score = 0;


let balls = [];
balls[0] = {
  radius : 10,
       x : 300,
       y : 300,
      dx : 2,
      dy : -3,
      vx : 2 / 10,
      vy : -3 / 10,
   color : "#333c4e" 
}
let ball = {
  radius : 10,
       x : 300,
       y : 300,
      dx : 2,
      dy : -3,
      vx : 2 / 10,
      vy : -3 / 10,
   color : "#333c4e" 
}

let player = {
      height : 40,
       width : 40,
      radius : 40,
           x : 40,
           y : 40,
       color : "#333c4e" 
}

let obstacle = {
        x : 400,
        y : 250,
     size : 100,
    color : "#333c4e" 
}


let rightPressed = false;
let leftPressed = false;
let upPressed = false;
let downPressed = false;

const upTouch = document.getElementById("up-touch");
const downTouch = document.getElementById("down-touch");
const leftTouch = document.getElementById("left-touch");
const rightTouch = document.getElementById("right-touch");
const aTouch = document.getElementById("a-touch");
const bTouch = document.getElementById("b-touch");

leftTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      leftPressed = true;
})
rightTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      rightPressed = true;
})
upTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      upPressed = true;
})
downTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      downPressed = true;
})
aTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      shootBall();
})
bTouch.addEventListener("touchstart", e => {
      e.preventDefault();
      window.location.reload();
})

//STOP Touch Event
leftTouch.addEventListener("touchend", e => {
      e.preventDefault();
      leftPressed = false;
})
rightTouch.addEventListener("touchend", e => {
      e.preventDefault();
      rightPressed = false;
})
upTouch.addEventListener("touchend", e => {
      e.preventDefault();
      upPressed = false;
})
downTouch.addEventListener("touchend", e => {
      e.preventDefault();
      downPressed = false;
})
aTouch.addEventListener("touchend", e => {
      e.preventDefault();
})
bTouch.addEventListener("touchend", e => {
      e.preventDefault();
})

// Eventlisteners
document.addEventListener("keydown", keyDownHandler, false);
document.addEventListener("keyup", keyUpHandler, false);
document.addEventListener("touchstart", touchStartHandler, false);
document.addEventListener("touchend", touchEndHandler, false);



// TOUCH Handlers
function touchStartHandler(e) {
    console.log(e);
    [...e.changedTouches].forEach(touch => {
        
        document.getElementById("logT").innerHTML = parseInt(touch.pageX);
        document.getElementById("logT_Y").innerHTML = parseInt(touch.pageY);

    })
}
function touchEndHandler(e) {
        console.log(e);
}

// DOWN Presses
function keyDownHandler(e) {
    if(e.key == "Right" || e.key == "ArrowRight" || e.key == "d") {
        rightPressed = true;
      e.preventDefault();
    }
    if(e.key == "Left" || e.key == "ArrowLeft" || e.key == "a") {
        leftPressed = true;
      e.preventDefault();
    }

    if(e.key == "Up" || e.key == "ArrowUp" || e.key == "w") {
        upPressed = true;
      e.preventDefault();
    }
    if(e.key == "Down" || e.key == "ArrowDown" || e.key == "s"){
        downPressed = true;
      e.preventDefault();
    }
    if(e.key == "Shift" || e.key == "Control"){
    
      e.preventDefault();
        shootBall();
    }
}

// KEY Releases
function keyUpHandler(e) {
    if(e.key == "Right" || e.key == "ArrowRight" || e.key == "d") {
        rightPressed = false;
      e.preventDefault();
    }
    if(e.key == "Left" || e.key == "ArrowLeft" || e.key == "a") {
        leftPressed = false;
      e.preventDefault();
    }
    if(e.key == "Up" || e.key == "ArrowUp" || e.key == "w") {
        upPressed = false;
      e.preventDefault();
    }
    if(e.key == "Down" || e.key == "ArrowDown" || e.key == "s") {
        downPressed = false;
      e.preventDefault();
    }
}

function drawBackground() {
    ctx.beginPath();
    ctx.rect(0, 0,  canvas.width, canvas.height);
    ctx.fillStyle = bgColor;
    ctx.fill();
    ctx.closePath();
 
}

function drawScore() {
    ctx.font = "30px Verdana";
    // Create gradient
    var gradient = ctx.createLinearGradient(0, 0, canvas.width, 0);
    gradient.addColorStop("0"," magenta");
    gradient.addColorStop("0.5", "blue");
    gradient.addColorStop("1.0", "red");
    // Fill with gradient
    //ctx.fillStyle = gradient;
    var scoreNow = "Score: " + parseInt(score);
    ctx.fillText(scoreNow, 10, 90);
}

function drawBall() {
    ctx.beginPath();
    ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI*2);
    ctx.fillStyle = ball.color;
    ctx.fill();
    ctx.closePath();
    document.getElementById("logB").innerHTML = parseInt(ball.x);
    document.getElementById("logB_Y").innerHTML = parseInt(ball.y);
}

function drawBullets() {
  for (let bullet in balls){
    ctx.beginPath();
    ctx.arc(balls[bullet].x, balls[bullet].y, balls[bullet].radius, 0, Math.PI*2);
    ctx.fillStyle = balls[bullet].color;
    ctx.fill();
    ctx.closePath();
  }
}


function drawPaddle() {
    ctx.beginPath();
    ctx.arc(player.x, player.y, player.radius , 0, Math.PI*2);
    ctx.fillStyle = player.color;
    ctx.fill();
    ctx.closePath();
    document.getElementById("logP").innerHTML = player.x;
    document.getElementById("logP_Y").innerHTML = player.y;
}

function drawBorder(){
    ctx.beginPath();
    ctx.rect(obstacle.x, obstacle.y, obstacle.size, obstacle.size);
    ctx.fillStyle = obstacle.color;
    ctx.fill();
    ctx.closePath();
    }


function shootBall() {
    var sizes = [10, 20, 30, 40];
    var colors = ["#333c4e", "#7132a8", "#34a832", "#a83232",
        "#fcfc51", "#03fcfc", "#ff8800", "#00d0ff"];
    const yy = parseInt(Math.random() * sizes.length)
    const xx = parseInt(Math.random() * colors.length)
    balls.push( {
          radius : sizes[yy],
               x : player.x,
               y : (player.y - player.radius),
              dx : 2,
              dy : -3,
              vx : 2 / 10,
              vy : -3 / 10,
           color : colors[xx]
    })
    document.getElementById("logBullets").innerHTML = balls.length;
}

function isInBox(obj) {
    
    let toFarRight = ( obj.x - obj.radius + 5) > (obstacle.x + obstacle.size);
    let toFarLeft = ( obj.x + obj.radius - 5) < obstacle.x;
    let toHigh = (obj.y + obj.radius - 5) < obstacle.y;
    let toLow = (obj.y - obj.radius + 5) > (obstacle.y + obstacle.size);


    if(!toFarRight && !toFarLeft && !toHigh && !toLow){
      score += 1;
      return true;
    } else {
      return false;
    }
}

function moveIt(obj, time){
    obj.x += parseInt(obj.vx * time);
    obj.y += parseInt(obj.vy * time);
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawBackground();
    drawBorder();
    drawBall();
    drawBullets();
    drawPaddle();
    // drawScore();
}

function checkCollision(bb){
    // Check Collsion Ball and floor or seeling
    if(bb.x  > canvas.width - bb.radius || bb.x  < bb.radius) {
        bb.vx = -bb.vx;
    }
    // Check for Collision to wall left and right
    if(bb.y > canvas.height - bb.radius || bb.y < bb.radius) {
        bb.vy = -bb.vy;
    }
    if(isInBox(bb)){
        bb.vx *= -1;
        bb.vy *= -1;
    }
    return bb;

}

let t0, previousTimeStamp;
let done = false;

function step(timestamp) {
  if (t0 === undefined) {
      t0 = timestamp;
      }
  let total_elapsed = timestamp - t0;
  let dt = timestamp  - previousTimeStamp;

    // START of Game Logic referring to dt

    ball = checkCollision(ball);

    balls.forEach(function (item, index) {
            balls[index] = checkCollision(item);
    });

    /* // Check Collsion Ball and floor or seeling */
    /* if(ball.x  > canvas.width - ball.radius || ball.x  < ball.radius) { */
    /*     ball.vx = -ball.vx; */
    /* } */
    /* // Check for Collision to wall left and right */
    /* if(ball.y > canvas.height - ball.radius || ball.y < ball.radius) { */
    /*     ball.vy = -ball.vy; */
    /* } */
    /* if(isInBox(ball)){ */
    /*     ball.vx *= -1; */
    /*     ball.vy *= -1; */
    /* } */


    // Check for collision between ball and player i.e. paddle 
    if(player.x - player.radius < ball.x + ball.radius && 
        ball.x - ball.radius < (player.x + player.radius) && 
        player.y - player.radius < ball.y + ball.radius  && 
        ball.y - ball.radius < player.y + player.radius) {
          ball.vy *= -1;
          ball.vx *= -1;
    }
    

    // check for collision between player and a Square Obstacle of oSize
    if(rightPressed) {
        player.x += 5;
        if (isInBox(player)){
            player.x -= 5;
        }
        if (player.x + player.radius > canvas.width){
            player.x = canvas.width - player.radius;
        }
    }
      else if(leftPressed) {
        player.x -= 5;
        if (isInBox(player)){
            player.x += 5;
        }
        if (player.x < player.radius){
            player.x = player.radius;
        }
    }
    if(upPressed) {
        player.y -= 5;
        if (isInBox(player)){
            player.y += 5;
        }
        if (player.y < player.radius) {
            player.y = player.radius;
        }
    }
    if(downPressed) {
       player.y += 5;
        if (isInBox(player)){
            player.y -= 5;
        }
       if (player.y > canvas.height - player.radius) {
           player.y = canvas.height - player.radius;
       }
    }
   
    // move the Balls
    if(!isNaN(dt)) {
      moveIt(ball, dt);
      for (let b in balls) {
        moveIt(balls[b], dt);
      }
    }

    draw();

    // der timer hier ist besser als der normale Intervaltimer
    previousTimeStamp = timestamp
    window.requestAnimationFrame(step);
}
window.requestAnimationFrame(step);

</script>
