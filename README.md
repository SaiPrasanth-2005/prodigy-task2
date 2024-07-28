//stopwatch.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <link rel="stylesheet" href="style1.css">
</head>
<body>
    <div class="stopwatch">
        <div id="display">00:00:00.000</div>
        <div class="controls">
            <button id="startStopBtn">Start</button>
            <button id="resetBtn">Reset</button>
            <button id="lapBtn">Lap</button>
        </div>
        <div class="laps">
            <ul id="lapsList"></ul>
        </div>
    </div>
    <script src="script1.js"></script>
</body>
</html>

// script1.js
let startTime, updatedTime, difference, tInterval;
let running = false;

const display = document.getElementById('display');
const startStopBtn = document.getElementById('startStopBtn');
const resetBtn = document.getElementById('resetBtn');
const lapBtn = document.getElementById('lapBtn');
const lapsList = document.getElementById('lapsList');

function startStop() {
    if (!running) {
        startTime = new Date().getTime();
        tInterval = setInterval(updateTime, 1);
        startStopBtn.textContent = 'Stop';
        lapBtn.disabled = false;
        resetBtn.disabled = false;
        running = true;
    } else {
        clearInterval(tInterval);
        startStopBtn.textContent = 'Start';
        running = false;
    }
}

function reset() {
    clearInterval(tInterval);
    running = false;
    startStopBtn.textContent = 'Start';
    display.textContent = '00:00:00.000';
    lapsList.innerHTML = '';
    lapBtn.disabled = true;
    resetBtn.disabled = true;
}

function updateTime() {
    updatedTime = new Date().getTime();
    difference = updatedTime - startTime;

    let hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    let minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
    let seconds = Math.floor((difference % (1000 * 60)) / 1000);
    let milliseconds = Math.floor((difference % 1000));

    display.textContent = 
        (hours < 10 ? '0' + hours : hours) + ':' +
        (minutes < 10 ? '0' + minutes : minutes) + ':' +
        (seconds < 10 ? '0' + seconds : seconds) + '.' +
        (milliseconds < 10 ? '00' + milliseconds : milliseconds < 100 ? '0' + milliseconds : milliseconds);
}

function recordLap() {
    let lapItem = document.createElement('li');
    lapItem.textContent = display.textContent;
    lapsList.appendChild(lapItem);
}

startStopBtn.addEventListener('click', startStop);
resetBtn.addEventListener('click', reset);
lapBtn.addEventListener('click', recordLap);

lapBtn.disabled = true;
resetBtn.disabled = true;

//style1.css
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
}

.stopwatch {
    text-align: center;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

#display {
    font-size: 2em;
    margin-bottom: 20px;
}

.controls button {
    font-size: 1em;
    padding: 10px 20px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.controls button:hover {
    background-color: #ddd;
}

.laps {
    margin-top: 20px;
}

.laps ul {
    list-style-type: none;
    padding: 0;
}

.laps ul li {
    padding: 5px;
    border-bottom: 1px solid #ddd;
}
