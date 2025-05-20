# prodigy_WD_02
###HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Stopwatch</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h1>Stopwatch</h1>
    <div id="display">00:00:00</div>
    <div class="buttons">
      <button id="start">Start</button>
      <button id="pause">Pause</button>
      <button id="reset">Reset</button>
      <button id="lap">Lap</button>
    </div>
    <ul id="laps"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>


####CSS

body {
  font-family: Arial, sans-serif;
  background: #f9f9f9;
  text-align: center;
  padding-top: 50px;
}

.container {
  background: white;
  width: 300px;
  margin: auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

#display {
  font-size: 2.5em;
  margin: 20px 0;
}

.buttons button {
  padding: 10px 15px;
  margin: 5px;
  font-size: 1em;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button#start { background: #4CAF50; color: white; }
button#pause { background: #ff9800; color: white; }
button#reset { background: #f44336; color: white; }
button#lap   { background: #2196F3; color: white; }

#laps {
  text-align: left;
  margin-top: 20px;
  max-height: 150px;
  overflow-y: auto;
}


###Javascript
let startTime = 0;
let elapsedTime = 0;
let timerInterval;
const display = document.getElementById('display');
const lapsList = document.getElementById('laps');

function updateTime() {
  const time = Date.now() - startTime + elapsedTime;
  const date = new Date(time);
  const minutes = String(date.getUTCMinutes()).padStart(2, '0');
  const seconds = String(date.getUTCSeconds()).padStart(2, '0');
  const milliseconds = String(Math.floor(date.getUTCMilliseconds() / 10)).padStart(2, '0');
  display.textContent = `${minutes}:${seconds}:${milliseconds}`;
}

document.getElementById('start').onclick = function () {
  if (!timerInterval) {
    startTime = Date.now();
    timerInterval = setInterval(updateTime, 10);
  }
};

document.getElementById('pause').onclick = function () {
  if (timerInterval) {
    clearInterval(timerInterval);
    elapsedTime += Date.now() - startTime;
    timerInterval = null;
  }
};

document.getElementById('reset').onclick = function () {
  clearInterval(timerInterval);
  timerInterval = null;
  startTime = 0;
  elapsedTime = 0;
  display.textContent = "00:00:00";
  lapsList.innerHTML = '';
};

document.getElementById('lap').onclick = function () {
  if (timerInterval) {
    const lapItem = document.createElement('li');
    lapItem.textContent = display.textContent;
    lapsList.appendChild(lapItem);
  }
};
