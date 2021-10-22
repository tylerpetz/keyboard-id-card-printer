<script setup>
import { ref, computed, reactive } from 'vue'
import { ColorPicker } from 'vue-color-kit'
import 'vue-color-kit/dist/vue-color-kit.css'

const gradientDirections = ['to top', 'to left', 'to right', 'to bottom']
const fields = {
  buildName: 'Build Name',
  keyboardName: 'Keyboard Name',
  keycaps: 'Keycaps',
  switches: 'Switches',
  stabs: 'Stabilizers',
  moreText: 'More Text'
}
const card = ref({ ...fields })


const style = reactive({
  bgDirection: 'to left',
  bgColors: ['#555555'],
  textColor: '#ffffff',
  textOrientation: 'left',
  selectingColor: -1,
  colorToChange: {}
})

function print() {
  window.print()
}

function addStyle() {
  style.bgColors.push(style.bgColors.at(-1))
}

function removeStyle() {
  style.bgColors.splice(-1)
}

function changeColor(color = '') {
  style.colorToChange = color
}

function confirmColorChange(index, color) {
  // console.log(color, index, selectingColor.value, colorToChange.value)
  style.bgColors[index] = color
  style.selectingColor = -1
  style.colorToChange = {}
  // console.log(color, index, selectingColor.value, colorToChange.value)
}

const cardStyles = computed(() => ({
  background: style.bgColors.length > 1 ? `linear-gradient(${style.bgDirection},${style.bgColors.join(',')})` : style.bgColors[0],
  color: style.textColor,
  'text-align': style.textOrientation,
}))
</script>

<template>
  <main class="flex flex-row h-screen bg-white relative text-gray-700">
    <form class="flex flex-col w-1/4 print:hidden p-3 space-y-4">
      <h1 class="text-center text-xl font-bold text-gray-900 py-4">Keyboard ID Card Printer</h1>
      <div v-for="field in Object.entries(fields)" :key="field[0]">
        <label class="block text-sm font-medium text-gray-700">
          {{ field[1] }}
          <input type="text" placeholder="" v-model="card[field[0]]" class="mt-1 appearance-none block w-full px-3 py-2 border  rounded-md shadow-sm border-gray-300 placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
        </label>
      </div>
      <div class="grid grid-cols-2 gap-4">
        <label class="flex flex-col items-center text-sm font-medium">
          Background
          <div class="flex items-center flex-row w-full space-x-2">
            <ColorPicker
              v-show="style.selectingColor > -1"
              theme="light"
              :color="style.bgColors[selectingColor]"
              :sucker-hide="true"
              :colors-default="[]"
              @change-color="changeColor"
              class="border-box fixed top-4 right-4"
              style="width: 218px;"
            >
              <button 
                class="w-full text-center bg-green-400 hover:bg-green-600 text-white font-bold rounded-sm mt-4 p-1 shadow-sm"
                type="button"
                @click="confirmColorChange(style.selectingColor, style.colorToChange.hex)"
              >Confirm Change</button>
            </ColorPicker>
            <button 
              v-for="(color, index) in style.bgColors" :index="color"
              class="appearance-none shadow-sm h-10 w-full" 
              :style="`background: ${color}`"
              @click="style.selectingColor = index"
              type="button"
            >
              &nbsp;
            </button>
            <!-- <input tabindex="-1" v-for="(color, index) in style.bgColors" :index="color" type="color" v-model="style.bgColors[index]" class="bg-transparent appearance-none shadow-sm h-10 w-full" /> -->
            <button v-if="style.bgColors.length > 1 && style.bgColors.length < 4" @click.native="removeStyle" type="button">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </button>
            <button v-if="style.bgColors.length < 3" @click.native="addStyle" type="button">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </button>
          </div>
        </label>
        <label class="flex flex-col items-center text-sm font-medium">
          Gradient Direction
          <select v-model="style.bgDirection" class="w-full max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
            <option v-for="direction in gradientDirections" :key="direction">{{ direction }}</option>
          </select>
        </label>
        <label class="flex flex-col items-center text-sm font-medium text-gray-700">
          Text Color
          <input type="color" v-model="style.textColor" class="block appearance-none shadow-sm h-10">
        </label>
        <label class="flex flex-col items-center text-sm font-medium">
          Text Orientation
          <select v-model="style.textOrientation" class="w-full max-w-lg block focus:ring-indigo-500 focus:border-indigo-500 shadow-sm sm:max-w-xs sm:text-sm border-gray-300 rounded-md">
            <option value="left">left</option>
            <option value="center">center</option>
            <option value="right">right</option>
          </select>
        </label>
      </div>
    </form>
    <section class="w-3/4 bg-gray-200 flex flex-col items-center justify-center print:bg-white print:items-start print:justify-start">
      <div 
        class="card flex flex-col space-y-2 justify-center w-[502px] h-[325px] p-12 shadow-lg relative print:shadow-none print:p-16"
        :style="cardStyles"
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
    </footer>
  </main>
</template>
