<script setup>
    import { onMounted, reactive } from 'vue';

    let startDate, currentDate;

    const timerStates = {
        Stopped: 0,
        Paused: 1,
        Running: 2
    }

    const state = reactive({currentState: timerStates.Stopped, timerLabel: "0"});

    onMounted(() => {
        setInterval(function(){
            switch (state.currentState){
                case timerStates.Running: {
                    currentDate = new Date();
                    let totalSeconds = (currentDate - startDate) / 1000;
                    let options = {
                        second: '2-digit',
                        minute: ((totalSeconds >= 60) ? '2-digit' : undefined),
                        hour: ((totalSeconds >= 60*60) ? 'numeric' :undefined)
                    }
                    state.timerLabel = (new Date(currentDate - startDate)).toLocaleString('ru-RU', options);
                }; break;

                case timerStates.Paused: {
                    let newCurrentDate = new Date();
                    let delta = (newCurrentDate - currentDate);
                    startDate = new Date(startDate.getTime() + delta);
                    currentDate = newCurrentDate;
                }; break;
            }
        }, 10)
    });

    function start(){
        if (state.currentState === timerStates.Stopped){
            startDate = currentDate = new Date();
        }
        state.currentState = timerStates.Running;
    }
    function stop(){
        state.currentState = timerStates.Stopped;
        state.timerLabel = '0';
    }
    function pause(){
        state.currentState = timerStates.Paused;
    }
</script>

<template>
    <div class="main" :class="{active: state.currentState === timerStates.Running}">
        <div class="timer">
            {{ state.timerLabel }}
        </div>
        <div class="controls">
            <button type="button" @click="start" v-if="state.currentState !== timerStates.Running">
                <img src="../assets/buttons/play.png" />
            </button>
            <button type="button" @click="pause" v-if="state.currentState === timerStates.Running">
                <img src="../assets/buttons/pause.png" />
            </button>
            <button type="button" @click="stop">
                <img src="../assets/buttons/stop.png" />
            </button>
        </div>
    </div>
</template>

<style scoped>
    div.main{
        background: #9E9E9E;
        font-size: 22px;
        width: 225px;
        height: 120px;

        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: center;
        gap: 1px;
    }
    div.timer,
    div.controls{
        background: #696969;
        color: #9E9E9E;

        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-evenly;
        flex: 1;
    }
    div.main.active{
        background: #FFFFFF;
    }
    div.main.active div.timer{
        color: #FFFFFF;
    }
    div.main.active div.controls button{
        opacity: 100%;
        filter: brightness(1.75);
        color: inherit;
    }
    div button{
        padding: 0;
        width: 20px;
        height: 20px;
        font-size: 12pt;
        background: none;
        opacity: 70%;
        transition: opacity 0.5s ease-in-out;
        border: none;
    }
    div button:hover{
        opacity: 100%;
        border: none;
        text-shadow: 0 0 10px;
    }
</style>
