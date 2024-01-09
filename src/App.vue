<script setup>
import { ref, watch, watchEffect } from 'vue';

const remainingSeconds = ref(0)

function decreaseTimer (val) {
  if (remainingSeconds.value === 0) return
  remainingSeconds.value -= val
}

function increaseTimer (val) {
  if (remainingSeconds.value === 30) return
  remainingSeconds.value += val
}

const timerInProgress = ref(false)
let intervalId = null
function startTimer () {
  timerInProgress.value = true
  intervalId = window.setInterval(() => {
    remainingSeconds.value -= 1;
  }, 1000)
}

function stopTimer () {
  clearInterval(intervalId)
  timerInProgress.value = false
}

// Same as watchEffect used below
// watch(
//   () => remainingSeconds.value,
//   (val) => {
//     if (val === 0 && timerInProgress.value) {
//       alert('Time\'s up !')
//       stopTimer()
//     }
//   }
// )

watchEffect(() => {
  if (remainingSeconds.value === 0 && timerInProgress.value) {
    stopTimer()
    alert('Time\'s up !')
  }
})
</script>

<template>
  <main>
    <h1>{{ remainingSeconds }}</h1>
    <div class="buttons">
      <div>
        <button :disabled="timerInProgress" class="time-button" @click="decreaseTimer(5)">-5</button>
        <button :disabled="timerInProgress" class="time-button" @click="increaseTimer(5)">+5</button>
      </div>
      <button class="toggle-button" @click="timerInProgress ? stopTimer() : startTimer()">
        {{ timerInProgress ? 'Stop' : 'Start' }}
      </button>
    </div>
  </main>
</template>

<style lang="postcss" scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  font-size: 80px;
}

.buttons {
  /* display: flex; */
  .time-button {
    background-color: #0476ef;
    border: none;
    border-radius: 100%;
    font-weight: 600;
    color: white;
    width: 40px;
    height: 40px;
    &:first-of-type {
      margin-right: 16px;
    }
  }

  .toggle-button {
    width: 100%;
    background-color: rgb(39, 123, 39);
    border-radius: 8px;
    color: white;
    border: none;
    margin-top: 8px;
    height: 32px;
  }
}
</style>