# Cronômetro de Estudo

<div id="timer">00:00:00</div>
<a href="#" onclick="startTimer()" class="md-button md-button--primary">Iniciar</a>
<a href="#" onclick="pauseTimer()" class="md-button md-button--secondary">Pausar</a>
<a href="#" onclick="resetTimer()" class="md-button md-button--danger">Resetar</a>

<script>
    let timer;
    let seconds = 0;
    let isRunning = false;

    function startTimer() {
        if (!isRunning) {
            isRunning = true;
            timer = setInterval(updateTime, 1000); 
        }
    }

    function pauseTimer() {
        if (isRunning) {
            clearInterval(timer);
            isRunning = false;
        }
    }

    function resetTimer() {
        clearInterval(timer);
        isRunning = false;
        seconds = 0;
        updateDisplay();
    }

    function updateTime() {
        seconds++;
        updateDisplay();
    }

    function updateDisplay() {
        let hours = Math.floor(seconds / 3600);
        let minutes = Math.floor((seconds % 3600) / 60);
        let secs = seconds % 60;
        document.getElementById('timer').textContent = 
            `${pad(hours)}:${pad(minutes)}:${pad(secs)}`;
    }

    function pad(value) {
        return value.toString().padStart(2, '0');
    }
</script>

<style>
    #timer {
        font-size: 2em;
        font-weight: bold;
        margin-bottom: 10px;
        text-align: center;
    }
    .md-button {
        font-size: 1em;
        margin: 5px;
        padding: 10px 20px;
        text-decoration: none;
        display: inline-block;
        border-radius: 4px;
        color: #fff;
        text-align: center;
    }
    .md-button--primary {
        background-color: #green;

    }
    .md-button--secondary {
        background-color: #grey;
    }
    .md-button--danger {
        background-color: red;
    }
    .md-button:hover {
        opacity: 0.9;
    }
</style>
