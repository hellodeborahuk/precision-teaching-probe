<script setup>
import { ref } from "vue";
import headingImage from '../assets/heading.png';

// Global mode
const mode = ref("text"); // 'text' | 'fraction'

// Inputs
const inputs = ref([
  { value: "" },
  { value: "" },
  { value: "" },
  { value: "" },
  { value: "" },
]);

const grid = ref([]);

// Shuffle without clumps
function shuffleNoClumps(array) {
  let result = [];

  while (array.length) {
    let index = Math.floor(Math.random() * array.length);

    if (result.length > 0 && array[index] === result[result.length - 1]) {
      continue;
    }

    result.push(array[index]);
    array.splice(index, 1);
  }

  return result;
}

// Generate balanced grid
function generateGrid() {
  const values = inputs.value.map((item) => ({
    ...item,
    type: mode.value,
  }));

  const firstRow = [...values];

  let pool = [];
  values.forEach((item) => {
    for (let i = 0; i < 7; i++) {
      pool.push(item);
    }
  });

  const shuffledPool = shuffleNoClumps([...pool]);

  grid.value = [...firstRow, ...shuffledPool];
}

// Print
function printGrid() {
  window.print();
}
</script>

<template>
  <div class="container rounded-2xl bg-blue-50 shadow-blue-300 print:shadow-none shadow-xl py-4 print:py-0">
    <!-- <h1 class="no-print mt-4 mb-8 print:m-0 text-6xl text-gray-700">Probe Creator</h1> -->
     <div class="w-full md:w-3/5 mx-auto pb-4 print:p-0 no-print">
      <img :src="headingImage" alt="Precision Teaching Probe Generator" />
    </div>  
    <div class="no-print rounded-2xl border-2 w-full md:w-2/3 mx-6 md:mx-auto border-blue-200 shadow-xl shadow-blue-200 p-8 bg-white">
  <div class="no-print relative flex pb-4 items-center">
    <div class="flex-grow border-t border-2 border-gray-300"></div>
    <span class="flex-shrink mx-4 text-gray-700 font-bold text-3xl">Enter your 5 items below</span>
    <div class="flex-grow border-t border-2 border-gray-300"></div>
  </div>
      <!-- INPUTS -->
      <div class="no-print inputs w-full md:w-1/2 lg:w-1/3 mx-auto space-y-4">
        <div v-for="(input, index) in inputs" :key="index" class="input-box placeholder:text-gray-400 flex items-center border-gray-100 border-2 rounded-lg p-1">
          <!-- TEXT -->
          <span class="text-base leading-none mr-1 select-none">✏️</span>
          <input
            v-if="mode === 'text'"
            v-model="input.value"
            placeholder="Enter value"
          />
        </div>
      </div>
          <div class="space-x-4 mt-6">
      <button class="bg-green-400 transition delay-150 duration-300 ease-in-out hover:bg-green-300 border-2 border-green-600 px-4 py-1 shadow-lg rounded-lg text-gray-800 font-semibold text-lg" @click="generateGrid">Generate Grid</button>
      <button class="bg-orange-400 transition delay-150 duration-300 ease-in-out hover:bg-orange-300 border-2 border-orange-600 px-4 py-1 shadow-lg rounded-lg text-gray-800 font-semibold text-lg" @click="printGrid">Print / Save PDF</button>
    </div>
    </div>

  <div class="no-print relative flex pt-4 print:p-0 items-center">
    <div class="flex-grow ml-4 border-t border-dotted border-4 border-blue-300"></div>
    <span class="flex-shrink mx-4 text-5xl">⭐</span>
    <div class="flex-grow mr-4 border-t border-dotted border-4 border-blue-300"></div>
  </div>

    
    <!-- HEADER -->
    <div class="text-center py-6 space-y-2">
      <h2 class="text-3xl font-semibold">Precision Teaching Probe</h2>
      <p>Name: ___________________ &nbsp;&nbsp;&nbsp; Date: _____________</p>
    </div>

    <!-- GRID -->
    <div id="grid-container">
   <div v-if="grid.length" id="grid-container" class="grid m-4">
  <div class="w-full border-l border-t border-black">
    <div
      v-for="row in 8"
      :key="row"
      class="flex w-full"
      style="height: 28mm;"
    >
      <div
        v-for="col in 5"
        :key="col"
        class="flex items-center justify-center border-r border-b border-black overflow-hidden"
        style="width: 20%; min-width: 0;"
      >
        <span class="text-center leading-tight px-1" style="font-size: clamp(12px, 2vw, 22px);">
          {{ grid[(row - 1) * 5 + (col - 1)]?.value ?? '' }}
        </span>
      </div>
    </div>
  </div>
</div>

  <!-- <table v-if="grid.length" class="grid">
    <tr v-for="row in 8" :key="row">
      <td v-for="col in 5" :key="col">
        <div class="cell-content">
          <template v-if="grid[(row - 1) * 5 + (col - 1)]">
            <span v-if="grid[(row - 1) * 5 + (col - 1)].type === 'text'">
              {{ grid[(row - 1) * 5 + (col - 1)].value }}
            </span>
          </template>
        </div>
      </td>
    </tr>
  </table> -->
</div>
  </div>
</template>

<style>
/* CONTAINER */
.container {
  max-width: 900px;
  margin: auto;
  text-align: center;
}

/* MODE BUTTONS */
.mode-buttons {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 15px;
}
.mode-buttons button {
  padding: 8px 12px;
  cursor: pointer;
}
.mode-buttons .active {
  background: black;
  color: white;
}

/* CELL CONTENT */
.cell-content {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  width: 100%;
  font-size: clamp(14px, 2vw, 22px);
  word-wrap: break-word;
}

/* PRINT */
@page {
  size: A4 portrait;
  margin: 10mm;
}

@media print {
  button,
  .inputs,
  .mode-buttons,
  h1,
  .no-print {
    display: none !important;
  }

  .container {
    padding: 0 !important;
    margin: 0 !important;
    max-width: 100% !important;
  }

  #grid-container {
    margin: 0 !important;
    padding: 0 !important;
  }
}
</style>