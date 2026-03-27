<script setup>
import { ref } from "vue";

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
  <div class="container rounded-2xl bg-blue-50">
    <h1 class="text-7xl">Precision Teaching Grid</h1>
<!-- 
    MODE BUTTONS
    <div class="mode-buttons">
      <button :class="{ active: mode === 'text' }" @click="mode = 'text'">
        Text / Number
      </button>
      <button
        :class="{ active: mode === 'fraction' }"
        @click="mode = 'fraction'"
      >
        Fraction
      </button>
    </div> -->

    <div class="rounded-2xl border-2 w-full md:w-2/3 mx-6 md:mx-auto m-10 border-blue-200 shadow-2xl shadow-blue-200 p-8 bg-white">
  <div class="relative flex pb-4 items-center">
    <div class="flex-grow border-t border-2 border-gray-300"></div>
    <span class="flex-shrink mx-4 text-gray-600 font-bold text-2xl">Enter your 5 items below</span>
    <div class="flex-grow border-t border-2 border-gray-300"></div>
  </div>
      <!-- INPUTS -->
      <div class="inputs w-full md:w-1/2 lg:w-1/3 mx-auto space-y-4">
        <div v-for="(input, index) in inputs" :key="index" class="input-box placeholder:text-gray-400 flex items-center border-gray-100 border-2 rounded-lg p-1">
          <!-- TEXT -->
          <span class="text-base leading-none mr-1 select-none">✏️</span>
          <input
            v-if="mode === 'text'"
            v-model="input.value"
            placeholder="Enter value"
          />

          <!-- FRACTION -->
          <div v-if="mode === 'fraction'" class="fraction-input">
            <input
              v-model="input.top"
              placeholder="Top"
              class="fraction-field"
            />
            <div class="fraction-line"></div>
            <input
              v-model="input.bottom"
              placeholder="Bottom"
              class="fraction-field"
            />
          </div>
        </div>
      </div>
    </div>
    <button @click="generateGrid">Generate Grid</button>
    <button @click="printGrid">Print / Save PDF</button>

    <!-- HEADER -->
    <div class="print-header">
      <h2>Practice Grid</h2>
      <p>Name: ____________ &nbsp;&nbsp;&nbsp; Date: ____________</p>
    </div>

    <!-- GRID -->
    <table v-if="grid.length" class="grid">
      <tr v-for="row in 8" :key="row">
        <td v-for="col in 5" :key="col">
          <div class="cell-content">
            <template v-if="grid[(row - 1) * 5 + (col - 1)]">
              <!-- TEXT -->
              <span v-if="grid[(row - 1) * 5 + (col - 1)].type === 'text'">
                {{ grid[(row - 1) * 5 + (col - 1)].value }}
              </span>

              <!-- FRACTION -->
              <div v-else class="fraction">
                <span>{{ grid[(row - 1) * 5 + (col - 1)].top }}</span>
                <div class="fraction-line"></div>
                <span>{{ grid[(row - 1) * 5 + (col - 1)].bottom }}</span>
              </div>
            </template>
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<style>


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

/* INPUTS */
/* .inputs {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
  justify-content: center;
} */

/* .input-box {
  display: flex;
  flex-direction: column;
  align-items: center;
} */

/* FRACTION INPUT */
.fraction-input {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 60px;
}

.fraction-field {
  width: 100%;
  text-align: center;
}

.fraction-line {
  width: 100%;
  height: 2px;
  background: black;
  margin: 2px 0;
}

/* HEADER */
.print-header {
  margin: 5mm 0;
}

.print-header h2 {
  margin: 0;
  font-size: 18px;
}

.print-header p {
  margin: 2mm 0 0;
  font-size: 14px;
}

/* GRID - SAFE PRINT VERSION */
.grid {
  border-collapse: collapse;
  width: 100%;
  table-layout: fixed;
}

.grid td {
  border: 1px solid black;
  width: 20%;
  height: 28mm; /* safe height */
  text-align: center;
  vertical-align: middle;
  padding: 2mm;
}

/* CONTENT */
.cell-content {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

/* TEXT */
.grid td span {
  font-size: clamp(14px, 2vw, 22px);
  word-wrap: break-word;
}

/* FRACTION DISPLAY */
.fraction {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: clamp(16px, 2vw, 24px);
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
  h1 {
    display: none;
  }
}
</style>
