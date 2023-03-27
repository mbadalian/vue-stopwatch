<script setup>
import { reactive } from 'vue';
import Stopwatch from './components/Stopwatch.vue';

const state = reactive({count: 1});
function addStopwatch(){
  state.count++;
}
</script>

<template>
  <div class="main-container">
    <Stopwatch v-for="i in state.count" />
    <button type="button" @click="addStopwatch">
      <img src="src/assets/buttons/add.png" />
    </button>
  </div>
</template>

<style scoped>
  div.main-container{
    /* display: flex;
    flex-wrap: wrap; */
    gap: 50px;

    display: grid;
    max-width: 320px;
    grid: auto-flow / repeat(auto-fit, minmax(225px, 1fr));
  }
  @media screen and (min-width: 768px) {
    div.main-container{
      max-width: 768px;
    }
  }
  @media screen and (min-width: 1024px) {
    div.main-container{
      max-width: 1024px;
    }
  }
  div.main-container > button{
    background: #696969;
    width: 225px;
    height: 120px;
    border-radius: 0;
  }
</style>
