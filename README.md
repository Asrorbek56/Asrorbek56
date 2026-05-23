<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=250&color=0:000000,50:8A2BE2,100:000000&text=Welcome%20to%20my%20profile!&fontSize=50&fontColor=39FF14&animation=twinkling&fontAlignY=38"/>
</p>

## Hi there 👋

<!--
**Asrorbek56/Asrorbek56** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gonka O'yini</title>

<style>
    body{
        margin:0;
        overflow:hidden;
        background:#222;
        font-family:Arial;
    }

    #game{
        width:400px;
        height:100vh;
        background:#555;
        margin:auto;
        position:relative;
        overflow:hidden;
        border-left:6px solid white;
        border-right:6px solid white;
    }

    .line{
        width:10px;
        height:100px;
        background:white;
        position:absolute;
        left:195px;
    }

    #car{
        width:50px;
        height:90px;
        background:red;
        position:absolute;
        bottom:20px;
        left:175px;
        border-radius:10px;
    }

    .enemy{
        width:50px;
        height:90px;
        background:yellow;
        position:absolute;
        top:-100px;
        border-radius:10px;
    }

    #score{
        position:absolute;
        top:20px;
        left:20px;
        color:white;
        font-size:24px;
        z-index:100;
    }

    #start{
        position:absolute;
        top:50%;
        left:50%;
        transform:translate(-50%,-50%);
        padding:15px 30px;
        font-size:24px;
        cursor:pointer;
        z-index:200;
    }
</style>
</head>

<body>

<div id="game">

    <div id="score">Score: 0</div>

    <button id="start">START</button>

    <div id="car"></div>

</div>

<script>

const game = document.getElementById("game");
const car = document.getElementById("car");
const scoreText = document.getElementById("score");
const startBtn = document.getElementById("start");

let score = 0;
let gameRunning = false;

let carX = 175;

document.addEventListener("keydown", moveCar);

function moveCar(e){

    if(!gameRunning) return;

    if(e.key === "ArrowLeft" && carX > 0){
        carX -= 25;
    }

    if(e.key === "ArrowRight" && carX < 350){
        carX += 25;
    }

    car.style.left = carX + "px";
}

function createLines(){

    for(let i=0;i<5;i++){

        let line = document.createElement("div");
        line.classList.add("line");

        line.style.top = (i*150) + "px";

        game.appendChild(line);
    }
}

function moveLines(){

    let lines = document.querySelectorAll(".line");

    lines.forEach(line => {

        let top = parseInt(line.style.top);

        top += 5;

        if(top > window.innerHeight){
            top = -100;
        }

        line.style.top = top + "px";
    });
}

function createEnemy(){

    let enemy = document.createElement("div");
    enemy.classList.add("enemy");

    enemy.style.left = Math.floor(Math.random()*350) + "px";

    game.appendChild(enemy);
}

function moveEnemy(){

    let enemies = document.querySelectorAll(".enemy");

    enemies.forEach(enemy => {

        let top = enemy.offsetTop;

        top += 6;

        enemy.style.top = top + "px";

        if(top > window.innerHeight){

            enemy.remove();

            score++;
            scoreText.innerText = "Score: " + score;

            createEnemy();
        }

        // urilish tekshirish
        let carRect = car.getBoundingClientRect();
        let enemyRect = enemy.getBoundingClientRect();

        if(
            carRect.left < enemyRect.right &&
            carRect.right > enemyRect.left &&
            carRect.top < enemyRect.bottom &&
            carRect.bottom > enemyRect.top
        ){
            alert("GAME OVER!\nScore: " + score);
            location.reload();
        }

    });
}

function gameLoop(){

    if(gameRunning){

        moveLines();
        moveEnemy();

        requestAnimationFrame(gameLoop);
    }
}

startBtn.addEventListener("click", () => {

    startBtn.style.display = "none";

    gameRunning = true;

    createLines();
    createEnemy();

    gameLoop();
});

</script>

</body>
</html>
