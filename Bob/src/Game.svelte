<script>
    import { onMount } from "svelte"
    import { createEventDispatcher } from 'svelte'

    const dispatch = createEventDispatcher()
    const gameSeconds = 60
    const bobTime = 700
    const bobSpeed = 3
    const bobWidth = 49
    const bobHeight = 79

    let canvas
    let ctx
    let bg
    let bob
    let score = 0
    let time = 0
    let bobX = 0
    let bobY = 0
    let bobW = 0
    let bobH = 0
    let bobXSeek
    let bobYSeek
    let bobWSeek
    let bobHSeek
    let gameTimerStop
    let drawTimerStop
    let bobTimerStop

    onMount(async () => {
        canvas = document.getElementById("canvas")
        ctx = canvas.getContext('2d')
        ctx.font = "bold 30px courier";
        loadBg()
    })

    function loadBg() {
        bg = new Image()
        bg.onload = function() {
            loadBob()
        }
        bg.src = 'background.png'
    }

    function loadBob() {
        bob = new Image()
        bob.onload = function() {
            start()
        }
        bob.src = 'bob.png'
    }

    function gameTimer() {
        time += 1
        if (time === gameSeconds) {
            dispatch("gameOver", {score: score})
            clearInterval(gameTimerStop)
            clearInterval(drawTimerStop)
            clearInterval(bobTimerStop)
        }
    }

    function moveBob() {
        bobXSeek = Math.random() * 600 + 20
        bobYSeek = Math.random() * 180 + 160
        const bobScale = bobYSeek / 200
        bobWSeek = bobWidth * bobScale
        bobHSeek = bobHeight * bobScale
    }

    function start() {
        moveBob()
        draw()
        gameTimerStop = window.setInterval(gameTimer, 1000)
        drawTimerStop = window.setInterval(draw, 50)
        bobTimerStop = window.setInterval(moveBob, bobTime)
        canvas.addEventListener('mousedown', (event) => { 
            new Audio('gunshot.mp3').play()
            const rect = canvas.getBoundingClientRect();
            const mouseX = event.x - rect.left
            const mouseY = event.y - rect.top
            if (mouseX > bobX && mouseX < bobX + bobW && mouseY > bobY && mouseY < bobY + bobH) {
                new Audio('scream.mp3').play()
                //console.log("bob hit!")
                score += 1
                clearInterval(bobTimerStop)
                bobTimerStop = window.setInterval(moveBob, bobTime)
                moveBob()
            }
        })
    }

    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height)
        ctx.drawImage(bg, 0, 0, canvas.width, canvas.height, 0, 0, canvas.width, canvas.height)
        ctx.drawImage(bob, 0, 0, bobWidth, bobHeight, bobX, bobY, bobW, bobH)
        //console.log(bobX, bobXSeek)
        bobX += (bobXSeek - bobX) / bobSpeed
        bobY += (bobYSeek - bobY) / bobSpeed
        bobW += (bobWSeek - bobW) / bobSpeed
        bobH += (bobHSeek - bobH) / bobSpeed
        ctx.fillText("Score: " + score, 30, 597)
        ctx.fillText("Time left:" + (((gameSeconds - time) < 10) ? "  " : " ") + (gameSeconds - time) + " Sec", 370, 597)
    }
    
</script>
<main>
    <canvas id="canvas" width="710" height="632" ></canvas>
</main>
<style>
    main {
        margin:0;
        cursor: url('./crosshair3.png'),auto;
    }
</style>