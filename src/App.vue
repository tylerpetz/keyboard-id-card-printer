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

const card = ref(fields)

function print() {
  window.print()
}
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
</script>

<template>
  <main class="flex flex-row h-screen bg-white">
    <form class="flex flex-col w-1/5 print:hidden p-3 space-y-4">
      <h1 class="text-center text-xl font-bold text-gray-900 py-4">Keyboard ID Card Printer</h1>
      <div v-for="field in Object.entries(fields)" :key="field[0]">
        <label for="email" class="block text-sm font-medium text-gray-700">
          {{ field[1] }}
        </label>
        <div class="mt-1">
          <input type="text" placeholder="Build Name" v-model="card[field[0]]" class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
        </div>
      </div>
    </form>
    <section class="w-4/5 bg-gray-200 flex flex-col items-center justify-center print:bg-white print:items-start print:justify-start">
      <div class="card flex flex-col space-y-2 justify-center bg-gradient-to-br from-[#7d23f3] to-[#7d23f3] w-[502px] h-[325px] p-12 shadow-lg text-white relative print:shadow-none print:p-16">
        <div class="border border-white absolute m-4 inset-0 border-dashed hidden print:block" />
        <div class="text-3xl pb-4 font-bold">{{ card.buildName }}</div>
        <div class="text-xl">{{ card.keyboardName }}</div>
        <div class="text-xl">{{ card.keycaps }}</div>
        <div class="text-xl">{{ card.switches }}</div>
        <div class="text-xl">{{ card.stabs }}</div>
        <div class="text-sm pt-6">{{ card.moreText }}</div>
      </div>
      <button class="mt-4 print:hidden" @click="print">Print</button>
    </section>
  </main>
</template>
