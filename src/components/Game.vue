<script setup lang="ts">
import { ref } from 'vue';

const tics = ref({
  aA: '',
  aB: '',
  aC: '',
  bA: '',
  bB: '',
  bC: '',
  cA: '',
  cB: '',
  cC: '',
})

const player = ref('One')
const gameOver = ref('')

const handlerReset = () => {
  tics.value = {
    aA: '',
    aB: '',
    aC: '',
    bA: '',
    bB: '',
    bC: '',
    cA: '',
    cB: '',
    cC: '',
  }
  player.value = 'One'
  gameOver.value = ''
}

const checkStatus = () => {
  const checkAllEmpties = Object.entries(tics.value).flatMap((entry) => entry[1]).filter(entry => entry === '')
  if (checkAllEmpties.length === 0) gameOver.value = 'No Winner'

  if (tics.value.aA && tics.value.aA === tics.value.aB && tics.value.aA === tics.value.aC) gameOver.value = player.value;
  if (tics.value.bA && tics.value.bA === tics.value.bB && tics.value.bA === tics.value.bC) gameOver.value = player.value;
  if (tics.value.cA && tics.value.cA === tics.value.cB && tics.value.cA === tics.value.cC) gameOver.value = player.value;
  if (tics.value.aA && tics.value.aA === tics.value.bA && tics.value.aA === tics.value.cA) gameOver.value = player.value;
  if (tics.value.aB && tics.value.aB === tics.value.bB && tics.value.aB === tics.value.cB) gameOver.value = player.value;
  if (tics.value.aC && tics.value.aC === tics.value.bC && tics.value.aC === tics.value.cC) gameOver.value = player.value;
  if (tics.value.aA && tics.value.aA === tics.value.bB && tics.value.aA === tics.value.cC) gameOver.value = player.value;
  if (tics.value.cA && tics.value.cA === tics.value.bB && tics.value.cA === tics.value.aC) gameOver.value = player.value;
}

const handleClick = (key: 'aA' | 'aB' | 'aC' | 'bA' | 'bB' | 'bC' | 'cA' | 'cB' | 'cC') => {
  if (gameOver.value) return
  if (tics.value[key] !== '') return
  tics.value[key] = player.value
  checkStatus()
  if (gameOver.value) return
  player.value = player.value === 'One' ? 'Two' : 'One'
}

const handleMessage = () => {
  if (gameOver.value === 'No Winner') return 'All tics are tacked, but winner!'
  if (gameOver.value === 'One' || gameOver.value === 'Two') return `Player ${player.value} wins!`
  return `It is player ${player.value === 'One' ? 'X' : 'O'}'s turn.`
}
</script>

<template>
  <div class="flex h-[100dvh] justify-center items-center flex-col">
    <div class="mb-8 flex gap-3">
      <h2 class="font-semibold text-2xl">{{ handleMessage() }}</h2>
      <button class="py-1 px-2 bg-green-500 rounded text-white" v-if="gameOver !== ''"
        @click="handlerReset()">Reset</button>
    </div>

    <div class="grid grid-cols-3 border-2 border-slate-600">
      <div class="w-[100px] h-[100px] bg-slate-200 text-black border-2 border-slate-600 flex items-center justify-center text-3xl"
        v-for="value, key in tics" :key="key" @click="handleClick(key)">
        {{ value === 'One' ? 'X' : value === 'Two' ? 'O' : '' }}
      </div>
    </div>
  </div>
</template>
