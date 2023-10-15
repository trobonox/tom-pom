<div class="main-pomo">
    <span class="timer-text">{minutes}:{secondString}</span>
    <button on:click={startTimer}>Start Timer</button>
    <p class="cycle-count">Cycle #{focusCycleNum}</p>
</div>
<p>Mode: {pomodoroTimerMode}</p>

<script lang="ts">
    import { tweened } from 'svelte/motion';

    interface IMinDict {
        [index: string]: number;
    }

    const modeMinutes = {
        "workMode": 25,
        "shortBreak": 5,
        "longBreak": 15
    } as IMinDict

    let focusCycleNum = 0;
    let pomodoroTimerMode: "workMode" | "shortBreak" | "longBreak" = "workMode";

    $: selectedModeMinutes = modeMinutes[pomodoroTimerMode];

    $: totalSeconds = selectedModeMinutes * 60; // TYPE NUMBER OF SECONDS HERE
    $: timer = tweened(totalSeconds);

    let timerRunning = false;

    const startTimer = () => {
        timerRunning = true;
    }
    
    setInterval(() => {
        if ($timer > 0 && timerRunning) $timer--;
        if ($timer <= 0 && timerRunning) endTimer();
    }, 1000);

    const endTimer = () => {
        if ($timer > 0) return;

        timerRunning = false;
        focusCycleNum++;

        if (pomodoroTimerMode === "workMode") {
            if (focusCycleNum % 4 === 0) {
                pomodoroTimerMode = "longBreak";
            }
            else {
                pomodoroTimerMode = "shortBreak";
            }
        }
        else {
            pomodoroTimerMode = "workMode";
        }
        
    }

    $: minutes = Math.floor($timer / 60);
    $: seconds = Math.floor($timer - minutes * 60);
    $: secondString = seconds === 0 ? "00" : seconds.toString().length === 1 ? `0${seconds}` : seconds.toString(); 
</script>

<style>
    .main-pomo {
        margin-top: -200px;
        padding: 3rem 6rem;
        border: 2px solid #2b2b2f;
        border-radius: 32px;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-items: center;
    }

    .timer-text {
        font-size: 8rem;
        font-weight: bolder;
    }

    p {
        margin-top: 12px;
        color: #c0c0ce;
    }

    button {
        padding: 1.25rem 2.5rem;
        background-color: #3950a3;
        border-radius: 8px;

        transition: background-color ease-in-out 0.3s;
    }

    button:hover {
        background-color: #4e6ddb;
        cursor: pointer;
    }
</style>