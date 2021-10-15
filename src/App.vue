<script setup>
import { ref } from 'vue'

const fields = {
  buildName: 'Build Name',
  keyboardName: 'Keyboard Name',
  keycaps: 'Keycaps',
  switches: 'Switches',
  stabs: 'Stabilizers',
  moreText: 'More Text'
}

const card = ref({ ...fields })
const style = ref({
  bgDirection: 'br',
  bgColor1: '#555555',
  bgColor2: '#000000',
  textColor: '#ffffff',
  textOrientation: 'left'
})

function print() {
  window.print()
}
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
</script>

<template>
  <main class="flex flex-row h-screen bg-white relative dark:bg-gray-900 text-gray-700 dark:text-gray-300">
    <form class="flex flex-col w-1/4 print:hidden p-3 space-y-4">
      <h1 class="text-center text-xl font-bold text-gray-900 dark:text-gray-200 py-4">Keyboard ID Card Printer</h1>
      <div v-for="field in Object.entries(fields)" :key="field[0]">
        <label class="block text-sm font-medium">
          {{ field[1] }}
          <input type="text" placeholder="" v-model="card[field[0]]" class="mt-1 appearance-none block w-full px-3 py-2 border  rounded-md shadow-sm border-gray-300 placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 dark:bg-gray-800 dark:border-gray-600 sm:text-sm">
        </label>
      </div>
      <div class="flex flex-row justify-evenly">
        <label class="flex flex-col items-center text-sm font-medium">
          Background
          <input type="color" v-model="style.bgColor1" class="block appearance-none shadow-sm h-10">
        </label>
        <label class="flex flex-col items-center text-sm font-medium">
          Text Color
          <input type="color" v-model="style.textColor" class="block appearance-none shadow-sm h-10">
        </label>
         <label class="flex flex-col items-center text-sm font-medium">
          Text Orientation
          <select v-model="style.textOrientation" class="max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
            <option value="left">left</option>
            <option value="center">center</option>
            <option value="right">right</option>
          </select>
        </label>
      </div>
      <!-- <div>
        <label class="block text-sm font-medium text-gray-700">
          Background 2
          <input type="color" v-model="style.bgColor2" class="block appearance-none shadow-sm">
        </label>
      </div> -->
    </form>
    <section class="w-3/4 bg-gray-200 dark:bg-gray-900 flex flex-col items-center justify-center print:bg-white print:items-start print:justify-start">
      <div 
        class="card flex flex-col space-y-2 justify-center w-[502px] h-[325px] p-12 shadow-lg relative print:shadow-none print:p-16"
        :style="`background-color: ${style.bgColor1}; color: ${style.textColor}; text-align: ${style.textOrientation}`"
      >
        <div class="border border-white absolute m-4 inset-0 border-dashed hidden print:block" />
        <div v-if="card.buildName" class="text-3xl pb-4 font-bold">{{ card.buildName }}</div>
        <div v-if="card.keyboardName" class="text-xl">{{ card.keyboardName }}</div>
        <div v-if="card.keycaps" class="text-xl">{{ card.keycaps }}</div>
        <div v-if="card.switches" class="text-xl">{{ card.switches }}</div>
        <div v-if="card.stabs" class="text-xl">{{ card.stabs }}</div>
        <div v-if="card.moreText" class="text-sm pt-6">{{ card.moreText }}</div>
      </div>
      <button class="mt-4 print:hidden" @click="print">Print</button>
    </section>
    <footer class="absolute right-0 bottom-0 text-xs p-4 flex flex-row items-center space-x-4">
      <a href="https://github.com/tylerpetz/keyboard-id-card-printer/issues" target="_blank">Issues</a>
      <button class="w-6 h-6 rounded-full bg-gray-200 text-gray-800 border border-gray-400 flex items-center justify-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
        </svg>
      </button>
    </footer>
  </main>
</template>
