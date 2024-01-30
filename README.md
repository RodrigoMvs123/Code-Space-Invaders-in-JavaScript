# Code-Space-Invaders-in-JavaScript

https://www.youtube.com/watch?v=s6LrpUTQQn0 

https://raw.githubusercontent.com/RodrigoMvs123/Code-Space-Invaders-in-JavaScript/main/README.md

https://github.com/RodrigoMvs123/Code-Space-Invaders-in-JavaScript/blame/main/README.md

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js

app.js

Visual Studio Code
Explorer 
Open Editors
index.html

index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Space Invaders</title>
</head>
<body>
    
    <div class="grid"></div>

    <script src="app.js"></script>
</body>
</html>

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

style.css

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Invaders</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1 class="results">0</h1>
    <div class="grid"></div>
    <script src="app.js"></script>
</body>
</html>

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

style.css
.grid {
    width: 300px;
    height: 300px;
    border: solid 1px black;
}

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

app.js
const grid = document.querySelector('.grid')
const resultDisplay = document.querySelector('.results')
const width = 15

for (let i = 0; i < width * width; i++) {
    const square = document.createElement('div')
    grid.appendChild(square)
}

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

style.css
.grid {
    width: 300px;
    height: 300px;
    border: solid 1px black;
    display: flex;
    flex-wrap: wrap;
}

.grid div {
    width: 20px;
    height: 20px;
}

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

app.js
const grid = document.querySelector('.grid')
const resultDisplay = document.querySelector('.results')
const width = 15

for (let i = 0; i < width * width; i++) {
    const square = document.createElement('div')
    grid.appendChild(square)
}

const squares = Array.from(document.querySelectorAll('.grid div'))

console.log(squares)

const alienInvaders = [
    0,1,2,3,4,5,6,7,8,9,
    15,16,17,18,19,20,21,22,23,24,
    30,31,32,33,34,35,36,37,38,39
]

function draw() {
    for (let i = 0; i < alienInvaders.length; i++) {
        squares[alienInvaders[i]].classList.add('invader')
    }
}

draw()

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

style.css
.grid {
    width: 300px;
    height: 300px;
    border: solid 1px black;
    display: flex;
    flex-wrap: wrap;
}

.grid div {
    width: 20px;
    height: 20px;
}

.invader {
    background-color: purple;
    border-radius: 10px;
}

.shooter {
    background-color: green;
}

.laser {
    background-color: orange;
}

.boom {
    background-color: red;
}

Visual Studio Code
Explorer 
Open Editors 
index.html
app.js
style.css

app.js
const grid = document.querySelector('.grid')
const resultDisplay = document.querySelector('.results')
let currentShooterIndex = 202
const width = 15
const aliensRemoved = []
let invadersId
let isGoingRight = true
let direction = 1
let results = 0 
    
for (let i = 0; i < width * width; i++) {
    const square = document.createElement('div')
    grid.appendChild(square)
}

const squares = Array.from(document.querySelectorAll('.grid div'))

console.log(squares)

const alienInvaders = [
    0,1,2,3,4,5,6,7,8,9,
    15,16,17,18,19,20,21,22,23,24,
    30,31,32,33,34,35,36,37,38,39
]

function draw() {
    for (let i = 0; i < alienInvaders.length; i++) {
        if (!aliensRemoved.includes(i)) {
            squares[alienInvaders[i]].classList.add('invader')
        }
    }
}
draw()

squares[currentShooterIndex].classList.add('shooter')

function remove() {
    for (let i = 0; i < alienInvaders.length; i++) {
        squares[alienInvaders[i]].classList.remove('invader')
    }
}

function moveShooter(e) {
    squares[currentShooterIndex].classList.remove('shooter')
    switch(e.key) {
        case 'ArrowLeft':
            if (currentShooterIndex % width !==0) currentShooterIndex -=1
            break 
        case 'ArrowRight':
            if (currentShooterIndex % width < width - 1) currentShooterIndex +=1
            break
    }
    squares[currentShooterIndex].classList.add('shooter')
}

document.addEventListener('keydown', moveShooter)

function moveInvaders() {
    const leftEdge = alienInvaders[0] % width === 0
    const rightEdge = alienInvaders[alienInvaders.length - 1] % width === width - 1
    remove()

    if (rightEdge && isGoingRight) {
        for (let i = 0; i < alienInvaders.length; i++) {
            alienInvaders[i] += width + 1
            direction = -1
            isGoingRight = false
        }        
    }

    if (leftEdge &&!isGoingRight) {
        for (let i = 0; i < alienInvaders.length; i++) {
            alienInvaders[i] += width - 1
            direction = 1
            isGoingRight = true
        }
    }

    for (let i = 0; i < alienInvaders.length; i++) {
        alienInvaders[i] += direction
    }
    draw()

    if (squares[currentShooterIndex].classList.contains("invader")) {
        resultDisplay.innerHTML = 'GAME OVER'
        clearInterval(invadersId)
    }

    if (aliensRemoved.length === alienInvaders.length) {
        resultDisplay.innerHTML = 'YOU WIN'
        clearInterval(invadersId)
    }

}
invadersId = setInterval(moveInvaders, 600)

function shoot(e) {
    let laserId
    let currentLaserIndex = currentShooterIndex

    function moveLaser() {
        squares[currentLaserIndex].classList.remove('laser')
        currentLaserIndex -= width
        squares[currentLaserIndex].classList.add('laser')

        if (squares[currentLaserIndex].classList.contains('invader')) {
            squares[currentLaserIndex].classList.remove('laser')
            squares[currentLaserIndex].classList.remove('invader')
            squares[currentLaserIndex].classList.add('boom')

            setTimeout(() => squares[currentLaserIndex].classList.remove('boom'), 300)
            clearInterval(laserId)

            const alienRemoved = alienInvaders.indexOf(currentLaserIndex)
            aliensRemoved.push(alienRemoved)
            results++
            resultDisplay.innerHTML = results
            console.log(aliensRemoved)

        }

    }

    if (e.key === 'ArrowUp') {
        laserId = setInterval(moveLaser, 100)
    }
}

document.addEventListener('keydown', shoot)











