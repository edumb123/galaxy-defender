<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galaxy Defender</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
        }
        canvas {
            display: block;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        class Player {
            constructor() {
                this.x = canvas.width / 2;
                this.y = canvas.height - 50;
                this.width = 50;
                this.height = 50;
                this.speed = 5;
            }
            draw() {
                ctx.fillStyle = "blue";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            }
            move(direction) {
                if (direction === "left" && this.x > 0) this.x -= this.speed;
                if (direction === "right" && this.x < canvas.width - this.width) this.x += this.speed;
            }
        }

        class Asteroid {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = 0;
                this.width = 50;
                this.height = 50;
                this.speed = Math.random() * 3 + 2;
            }
            draw() {
                ctx.fillStyle = "red";
                ctx.fillRect(this.x, this.y, this.width, this.height);
            }
            update() {
                this.y += this.speed;
            }
        }

        const player = new Player();
        const asteroids = [];
        let gameOver = false;

        function spawnAsteroid() {
            if (!gameOver) {
                asteroids.push(new Asteroid());
                setTimeout(spawnAsteroid, 1000);
            }
        }
        
        function updateGame() {
            if (gameOver) return;

            ctx.clearRect(0, 0, canvas.width, canvas.height);
            player.draw();

            asteroids.forEach((asteroid, index) => {
                asteroid.update();
                asteroid.draw();
                if (
                    asteroid.y + asteroid.height >= player.y &&
                    asteroid.x < player.x + player.width &&
                    asteroid.x + asteroid.width > player.x
                ) {
                    gameOver = true;
                    alert("Game Over!");
                }
                if (asteroid.y > canvas.height) {
                    asteroids.splice(index, 1);
                }
            });

            requestAnimationFrame(updateGame);
        }

        document.addEventListener("keydown", (event) => {
            if (event.key === "ArrowLeft") player.move("left");
            if (event.key === "ArrowRight") player.move("right");
        });

        spawnAsteroid();
        updateGame();
    </script>
</body>
</html>
