<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Stopwatch</title>
    <style>
        body { font-family: Arial, sans-serif; background: linear-gradient(120deg, #89f7fe, #66a6ff); height: 100vh; display: flex; align-items: center; justify-content: center; }
        .stopwatch { background-color: #ffffffee; padding: 30px; border-radius: 20px; box-shadow: 0px 0px 20px #00000033; text-align: center; }
        .time { font-size: 3rem; margin-bottom: 20px; font-weight: bold; color: #333; }
        button { padding: 10px 20px; margin: 5px; font-size: 1rem; border-radius: 10px; border: none; cursor: pointer; transition: background-color 0.3s; }
        button:hover { background-color: #66a6ff; color: #fff; }
        .laps { margin-top: 20px; max-height: 150px; overflow-y: auto; padding: 10px; border: 1px solid #ccc; border-radius: 10px; background: #f0f0f0; }
        .lap-item { padding: 5px 0; font-size: 1rem; }
        .btn-group { display: flex; justify-content: center; }
    </style>
</head>
<body>
    <div class="stopwatch">
        <div class="time" id="display">00:00:00.00</div>
        <div class="btn-group">
            <button onclick="startStopwatch()">Start</button>
            <button onclick="pauseStopwatch()">Pause</button>
            <button onclick="resetStopwatch()">Reset</button>
            <button onclick="lapTime()">Lap</button>
        </div>
        <div class="laps" id="laps"></div>
    </div>

    <script>
        let timerInterval, elapsedTime = 0, running = false, lapCount = 0;
        function formatTime(ms) {
            const milliseconds = Math.floor(ms % 1000 / 10);
            const seconds = Math.floor(ms / 1000) % 60;
            const minutes = Math.floor(ms / 60000) % 60;
            const hours = Math.floor(ms / 3600000);
            return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}.${String(milliseconds).padStart(2, '0')}`;
        }
        function startStopwatch() {
            if (!running) {
                running = true;
                const startTime = Date.now() - elapsedTime;
                timerInterval = setInterval(() => {
                    elapsedTime = Date.now() - startTime;
                    document.getElementById('display').innerText = formatTime(elapsedTime);
                }, 10);
            }
        }
        function pauseStopwatch() {
            running = false;
            clearInterval(timerInterval);
        }
        function resetStopwatch() {
            running = false;
            clearInterval(timerInterval);
            elapsedTime = 0;
            lapCount = 0;
            document.getElementById('display').innerText = "00:00:00.00";
            document.getElementById('laps').innerHTML = "";
        }
        function lapTime() {
            if (running) {
                lapCount++;
                const lap = document.createElement('div');
                lap.className = 'lap-item';
                lap.innerText = `Lap ${lapCount}: ${formatTime(elapsedTime)}`;
                document.getElementById('laps').appendChild(lap);
            }
        }
    </script>
</body>
</html>
