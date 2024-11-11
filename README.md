// Game setup
let score = 0;
let gameInterval;
let character = document.getElementById('character');
let coin = document.getElementById('coin');
let hurdle = document.getElementById('hurdle');
let scoreBoard = document.getElementById('score-board');
let gameContainer = document.querySelector('.game-container');

let jumpHeight = 100;  // How high the character jumps
let gravity = 2;       // How fast the character falls
let characterSpeed = 5; // Speed at which character moves horizontally

let isJumping = false;
let isGameOver = false;

// Initial positions
coin.style.left = `${Math.random() * 750}px`;
hurdle.style.left = `${Math.random() * 750}px`;

// Event listener for jump action
document.addEventListener('keydown', jump);

function jump(e) {
    if (e.key === ' ' && !isJumping && !isGameOver) {
        isJumping = true;
        let jumpHeightCurrent = 0;
        
        let jumpUp = setInterval(() => {
            if (jumpHeightCurrent < jumpHeight) {
                character.style.bottom = `${parseInt(character.style.bottom) + 10}px`;
                jumpHeightCurrent += 10;
            } else {
                clearInterval(jumpUp);
                let fallDown = setInterval(() => {
                    if (parseInt(character.style.bottom) > 0) {
                        character.style.bottom = `${parseInt(character.style.bottom) - 10}px`;
                    } else {
                        clearInterval(fallDown);
                        isJumping = false;
                    }
                }, gravity);
            }
        }, 15);
    }
}

// Game loop
gameInterval = setInterval(() => {
    if (isGameOver) {
        clearInterval(gameInterval);
        alert('Game Over! Your score: ' + score);
        return;
    }

    // Move coin and hurdle
    coin.style.left = `${parseInt(coin.style.left) - 5}px`;
    hurdle.style.left = `${parseInt(hurdle.style.left) - 5}px`;

    // Reset coin and hurdle if off screen
    if (parseInt(coin.style.left) < 0) {
        coin.style.left = `${Math.random() * 750}px`;
    }
    if (parseInt(hurdle.style.left) < 0) {
        hurdle.style.left = `${Math.random() * 750}px`;
    }

    // Check for collisions with coin
    if (parseInt(coin.style.left) < parseInt(character.style.left) + 50 && 
        parseInt(coin.style.left) + 30 > parseInt(character.style.left) && 
        parseInt(coin.style.bottom) < parseInt(character.style.bottom) + 70) {
        score += 10;
        coin.style.left = `${Math.random() * 750}px`;  // Reset coin
    }

    // Check for collisions with hurdle
    if (parseInt(hurdle.style.left) < parseInt(character.style.left) + 50 && 
        parseInt(hurdle.style.left) + 40 > parseInt(character.style.left) &&
        parseInt(hurdle.style.bottom) < parseInt(character.style.bottom) + 70) {
        isGameOver = true;
    }

    // Update score
    scoreBoard.textContent = `Score: ${score}`;

    // Change background every 1000 points
    if (score >= 1000 && score < 2000) {
        gameContainer.className = 'game-container bg-spring';
    } else if (score >= 2000 && score < 3000) {
        gameContainer.className = 'game-container bg-summer';
    } else if (score >= 3000 && score < 4000) {
        gameContainer.className = 'game-container bg-fall';
    } else if (score >= 4000) {
        gameContainer.className = 'game-container bg-winter';
    }

}, 100);
