<!DOCTYPE html>
<html>
<head>
    <title>Space Invaders</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
            background-color: black;
            color: white;
        }
        #gameArea {
            margin: 20px auto;
            width: 600px;
            height: 400px;
            background-color: #111;
            position: relative;
            overflow: hidden;
            border: 2px solid white;
        }
        .player {
            width: 40px;
            height: 40px;
            background-color: lime;
            position: absolute;
            bottom: 10px;
            left: 280px;
        }
        .enemy {
            width: 30px;
            height: 30px;
            background-color: red;
            position: absolute;
        }
        .bullet {
            width: 5px;
            height: 10px;
            background-color: yellow;
            position: absolute;
        }
    </style>
</head>
<body>
    <h1>Space Invaders</h1>
    <p>Użyj klawiszy strzałek do poruszania i spacji do strzału</p>
    <div id="gameArea">
        <div id="player" class="player"></div>
    </div>
    <script>
        let player = document.getElementById("player");
        let gameArea = document.getElementById("gameArea");
        let enemies = [];
        let bullets = [];
        let playerSpeed = 10;
        let bulletSpeed = 5;
        let enemySpeed = 1;

        document.addEventListener("keydown", function(event) {
            let playerPos = player.offsetLeft;
            if (event.key === "ArrowLeft" && playerPos > 0) {
                player.style.left = (playerPos - playerSpeed) + "px";
            } else if (event.key === "ArrowRight" && playerPos < gameArea.clientWidth - 40) {
                player.style.left = (playerPos + playerSpeed) + "px";
            } else if (event.key === " ") {
                shoot();
            }
        });

        function shoot() {
            let bullet = document.createElement("div");
            bullet.classList.add("bullet");
            bullet.style.left = (player.offsetLeft + 17) + "px";
            bullet.style.bottom = "50px";
            gameArea.appendChild(bullet);
            bullets.push(bullet);
        }

        function moveBullets() {
            bullets.forEach((bullet, index) => {
                let bulletPos = parseInt(bullet.style.bottom);
                if (bulletPos > gameArea.clientHeight) {
                    bullet.remove();
                    bullets.splice(index, 1);
                } else {
                    bullet.style.bottom = (bulletPos + bulletSpeed) + "px";
                }
            });
        }

        function createEnemy() {
            let enemy = document.createElement("div");
            enemy.classList.add("enemy");
            enemy.style.left = Math.random() * (gameArea.clientWidth - 30) + "px";
            enemy.style.top = "0px";
            gameArea.appendChild(enemy);
            enemies.push(enemy);
        }

        function moveEnemies() {
            enemies.forEach((enemy, index) => {
                let enemyPos = parseInt(enemy.style.top);
                if (enemyPos > gameArea.clientHeight) {
                    enemy.remove();
                    enemies.splice(index, 1);
                } else {
                    enemy.style.top = (enemyPos + enemySpeed) + "px";
                }
            });
        }

        function gameLoop() {
            moveBullets();
            moveEnemies();
            requestAnimationFrame(gameLoop);
        }

        setInterval(createEnemy, 2000);
        gameLoop();
    </script>
</body>
</html>
