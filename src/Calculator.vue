<script setup lang="ts">
import { ref } from "vue";

const display = ref("");
const operatorClicked = ref(false);
const operator = ref<((a: number, b: number) => number) | null>(null);
const previousClick = ref<number | null>(null);

function clear() {
  display.value = "";
  operator.value = null;
  previousClick.value = null;
}

function getDigit(digit: string) {
  if (operatorClicked.value) {
    display.value = "";
    operatorClicked.value = false;
  }
  display.value += digit;
}

function plusminus() {
  if (display.value !== "") {
    display.value =
      display.value.charAt(0) === "-"
        ? display.value.slice(1)
        : `-${display.value}`;
  }
}

function percentage() {
  if (display.value) {
    display.value = (parseFloat(display.value) / 100).toString();
    previousClick.value = parseFloat(display.value);
    operatorClicked.value = true;
  }
}

function divide() {
  operator.value = (a, b) => a / b;
  previousClick.value = parseFloat(display.value);
  operatorClicked.value = true;
}

function multiply() {
  operator.value = (a, b) => a * b;
  previousClick.value = parseFloat(display.value);
  operatorClicked.value = true;
}

function minus() {
  operator.value = (a, b) => a - b;
  previousClick.value = parseFloat(display.value);
  operatorClicked.value = true;
}

function add() {
  operator.value = (a, b) => a + b;
  previousClick.value = parseFloat(display.value);
  operatorClicked.value = true;
}

function equal() {
  if (operator.value && previousClick.value !== null) {
    display.value = operator
      .value(previousClick.value, parseFloat(display.value))
      .toString();
    previousClick.value = null;
    operator.value = null;
  }
}

function dott() {
  if (!display.value.includes(".")) {
    display.value += ".";
  }
}
</script>

<template>
  <div class="container">
    <div class="display">{{ display || 0 }}</div>

    <div @click="clear" class="operands">AC</div>
    <div @click="plusminus" class="operands">+/-</div>
    <div @click="percentage" class="operands">%</div>

    <div @click="divide" class="operands">/</div>
    <div @click="getDigit('7')" class="digits">7</div>
    <div @click="getDigit('8')" class="digits">8</div>
    <div @click="getDigit('9')" class="digits">9</div>

    <div @click="multiply" class="operands">*</div>
    <div @click="getDigit('4')" class="digits">4</div>
    <div @click="getDigit('5')" class="digits">5</div>
    <div @click="getDigit('6')" class="digits">6</div>

    <div @click="minus" class="operands">-</div>
    <div @click="getDigit('1')" class="digits">1</div>
    <div @click="getDigit('2')" class="digits">2</div>
    <div @click="getDigit('3')" class="digits">3</div>

    <div @click="add" class="operands">+</div>
    <div @click="getDigit('0')" class="zero digits">0</div>
    <div @click="dott" class="digits">.</div>
    <div @click="equal" class="digits">=</div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=MedievalSharp&display=swap");
.container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  max-width: 400px;
  margin: 0 auto;
  background: #3b2f2f;
  border: 8px solid #d4a373;
  border-radius: 15px;
  box-shadow: 0 0 20px #000;
  font-family: "MedievalSharp", cursive; /* Assuming you include a medieval font */
}

.display {
  grid-column: span 4;
  background: url("https://www.transparenttextures.com/patterns/old-mathematics.png");
  color: #f5deb3;
  font-size: 2.5em;
  text-align: right;
  padding: 0.8em;
  border-bottom: 4px solid #d4a373;
  box-shadow: inset 0 0 10px #000;
  letter-spacing: 2px;
}

.digits,
.operands {
  text-align: center;
  font-size: 1.8em;
  padding: 1em;
  border: 1px solid #593c1f;
  cursor: pointer;
  color: #d4a373;
  background: linear-gradient(145deg, #6b4f4f, #4f3939);
  box-shadow: 0 0 8px #000;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.digits:hover,
.operands:hover {
  background: #4f3939;
  transform: translateY(-2px);
  box-shadow: 0 0 12px #000;
}

.digits:active,
.operands:active {
  transform: translateY(2px);
  box-shadow: inset 0 0 12px #000;
}

.digits::before,
.operands::before {
  content: "";
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  /* background: url("https://www.transparenttextures.com/patterns/arabesque.png"); */
  opacity: 0.1;
  pointer-events: none;
}

.digits::after,
.operands::after {
  content: "";
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  border: 1px solid #d4a373;
  box-sizing: border-box;
  pointer-events: none;
}

.zero {
  grid-column: span 2;
}
</style>
