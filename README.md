# Stopwatch Application

This repository contains a simple stopwatch application built with HTML, CSS, and JavaScript. The application allows users to start, stop, and reset a timer, displaying the elapsed time in minutes, seconds, and milliseconds. 

![image](https://github.com/LakinduNimesh/-Stopwatch-Application/assets/149768006/773426f2-6d15-4ae3-9b66-c302ebe3bff7)


## Files

- `index.html`: The main HTML file that contains the structure of the stopwatch.
- `style.css`: The CSS file that styles the stopwatch application.
- `script.js`: The JavaScript file that provides the stopwatch functionality.

## Getting Started

To get a local copy up and running follow these simple steps:

### Prerequisites

Ensure you have a modern web browser installed on your computer.

### Installation

1. Clone the repository:

   ```sh
   git clone https://github.com/LakinduNimesh/-Stopwatch-Application.git
   ```

2. Open the `index.html` file in your web browser to view and interact with the stopwatch application.

## Usage

The stopwatch application consists of a timer display and three buttons: Start, Stop, and Reset.

- **Start**: Starts the timer.
- **Stop**: Stops the timer.
- **Reset**: Resets the timer to 00:00:00.

## HTML Structure (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="style.css">
  <title>Stopwatch</title>
</head>

<body>
  <main class="stopwatch" id="stopwatch">
    <h1>Stopwatch</h1>
    <div role="timer" aria-live="polite" id="timer">00:00:00</div>
    <div class="buttons">
      <button id="start">Start</button>
      <button id="stop" disabled>Stop</button>
      <button id="reset" disabled>Reset</button>
    </div>
  </main>

  <script src="script.js"></script>
</body>

</html>
```

## CSS Styling (`style.css`)

The CSS styles the stopwatch with a dark-themed background, center-aligned layout, and styled buttons.

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Roboto", sans-serif;
  background: #000;
  background-image: linear-gradient(rgba(24, 24, 24, 0.7), rgba(24, 24, 24, 0.244)), url(https://images.pexels.com/photos/1186851/pexels-photo-1186851.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  width: 100%;
  height: 100vh;
}

.stopwatch {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 26.25em;
  padding: 1.5em;
  color: #fff;
  font-family: "Roboto", sans-serif;
  background: rgba(153, 153, 153, 0.3);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 1.25em;
  border: 1px solid rgba(255, 255, 255, 0.381);
}

.stopwatch h1 {
  font-size: 3rem;
  margin: 20px;
  color: #fff;
  text-transform: uppercase;
}

#timer {
  font-size: 4rem;
  margin: 20px;
  color: #000000;
}

button {
  margin: 10px;
  padding: 10px;
  width: 80px;
  font-weight: 600;
  color: #2d2b2b;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
  border: none;
  border-radius: 20px;
  font-size: 1rem;
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
  transition: 0.4s;
}

#start {
  background-color: rgb(124, 206, 124);
}

#start:hover {
  background-color: rgb(92, 206, 92);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.589);
  color: #000;
}

#stop {
  background-color: rgb(206, 124, 124);
}

#stop:hover {
  background-color: rgb(206, 92, 92);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.589);
  color: #000;
}

#reset {
  background-color: rgb(124, 138, 206);
}

#reset:hover {
  background-color: rgb(92, 109, 206);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.589);
  color: #000;
}
```

## JavaScript Functionality (`script.js`)

The JavaScript code handles the logic for starting, stopping, and resetting the stopwatch timer.

```javascript
const INTERVAL_MS = 1000 / 60;
let timerID;
let lastTimerStartTime = 0;
let millisElapsedBeforeLastStart = 0;

const timer = document.getElementById("timer");
const startButton = document.getElementById("start");
const stopButton = document.getElementById("stop");
const resetButton = document.getElementById("reset");

startButton.addEventListener("click", startTimer);
stopButton.addEventListener("click", stopTimer);
resetButton.addEventListener("click", resetTimers);

function startTimer() {
  startButton.disabled = true;
  stopButton.disabled = false;
  resetButton.disabled = true;

  lastTimerStartTime = Date.now();
  timerID = setInterval(updateTimer, INTERVAL_MS);
}

function stopTimer() {
  startButton.disabled = false;
  stopButton.disabled = true;
  resetButton.disabled = false;

  millisElapsedBeforeLastStart += Date.now() - lastTimerStartTime;
  clearInterval(timerID);
}

function resetTimers() {
  resetButton.disabled = true;
  timer.textContent = "00:00:00";
  millisElapsedBeforeLastStart = 0;
}

function updateTimer() {
  const millisElapsed =
    Date.now() - lastTimerStartTime + millisElapsedBeforeLastStart;
  const secondsElapsed = millisElapsed / 1000;
  const minutesElapsed = secondsElapsed / 60;

  const millisText = formatNumber(millisElapsed % 1000, 3);
  const secondsText = formatNumber(Math.floor(secondsElapsed) % 60, 2);
  const minutesText = formatNumber(Math.floor(minutesElapsed), 2);
  timer.textContent = `${minutesText}:${secondsText}:${millisText}`;
}

function formatNumber(number, desiredLength) {
  const stringNumber = String(number);
  if (stringNumber.length > desiredLength) {
    return stringNumber.slice(0, desiredLength);
  }

  return stringNumber.padStart(desiredLength, "0");
}
```

## License

Distributed under the MIT License. See `LICENSE` for more information.

## Acknowledgments

- Background image from [Pexels](https://www.pexels.com/photo/person-walking-on-suspension-bridge-1186851/)
- Font from [Google Fonts](https://fonts.google.com/)

---

