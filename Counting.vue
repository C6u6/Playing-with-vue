<script setup lang="ts">

import sound from '../assets/soundeffects/silly-effect.mp3';
import { ref } from 'vue';

let currentSeconds = ref(100);
let intervalRef = ref(-1);
let secondsToTriggerSounds = ref({value: [0, undefined]}); // [0, undefined] is used for type inferrence
let stopCountAtZero = ref({value: false});

function checkIfTheValueIsZeroThenStopCounting() {
  if (stopCountAtZero.value && ([1,-1].includes(currentSeconds.value))) {console.log('zero');stop();return;}
};
function incrementOrDecrement(arg: string) {
  if (intervalRef) clearInterval(intervalRef.value);
  const interval = setInterval(() => {
    checkIfTheValueIsZeroThenStopCounting();
    if (secondsToTriggerSounds.value.value.includes(currentSeconds.value)) {
      makeASound();
    };
    if (arg == 'increment') {
      currentSeconds.value++;
    } else {
      currentSeconds.value--;
    };
  },1000);
  intervalRef.value = interval;
};
function onInput(string: string) {
  secondsToTriggerSounds.value.value = string.split(',').map(el => {
    try {
      return Number(el.trim()); 
    } catch {
      console.log(el.trim());
    };
  });
};
function makeASound() {
  const audio = new Audio(sound);
  audio.play();
};
function setStopCountingAtZero() {
  stopCountAtZero.value.value = !stopCountAtZero.value.value;
};
function stop() {
  clearInterval(intervalRef.value);
};
</script>

<template>
  <div>
    <form>
      <input type="number" placeholder="0:00" v-model="currentSeconds"/>
    </form>
    <span>
      <button @click="incrementOrDecrement('increment')">
        Start To Count Up
      </button>
      <button @click="incrementOrDecrement('decrement')">
        Start To Count Down
      </button>
      <button @click="stop">
        Stop Counting
      </button>
    </span>
    <div>
      <span>
        <input id='stop-at-zero' type="checkbox" @click="setStopCountingAtZero"/>
        <label for='stop-at-zero'>Stop counting at zero</label>
      </span>
      <span>
        <label for='make-a-sound'>Make a sound when counter at: </label>
        <input id='make-a-sound' type="text" @input="(event: any) => onInput(event.target.value)" placeholder='1, 2, 34, 100, 105' />
    </span>
    </div>
  </div>
</template>

<style>
  body {
		background-color: #6F7890;
  }
  button {
    background-color: #0F3BF0;
    border: none;
    border-radius: 8px; 
    color: white;
    font-size: 15px;
    font-weight: 600;
    padding: 10px;
  }
  button:hover {
		opacity: 50%;
  }
  div {
		display: flex;
    flex-direction: column;
    margin: auto;
    gap: 30px;
  }
  form {
		margin: auto;
  }
  input {
		border: none;
    border-radius: 50%;
    font-size: 40px;
    font-weight: 600;
    height: 100px;
    text-align: center;
    width: 100px; 
  }
  input[type=number]::-webkit-inner-spin-button, 
	input[type=number]::-webkit-outer-spin-button { 
  	-webkit-appearance: none; 
  	margin: 0;
	}
  input[type=number] {
  	-moz-appearance: textfield;
	}
  label {
		color: white;
    font-size: 18px;
  }
  span {
		display: flex;
    gap: 10px;
    margin: auto;
  }
  span input[type=checkbox] {
		height: 20px
  }
  span input[type=text] {
		height: 20px;
    border-radius: 8px;
    font-size: 15px;
    width: auto;
  }
</style>
