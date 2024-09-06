<script setup lang="ts">
import { ref } from "vue";

const result = ref(0);

const count1 = ref(0);
const count2 = ref(0);
const operation = ref("+");

const options = ["+", "-", "*", "/"];

defineProps<{ msg: string }>();

function count(count1: number, count2: number, operation: string) {
  switch (operation) {
    case "+":
      result.value = count1 + count2;
      break;
    case "-":
      result.value = count1 - count2;
      break;
    case "*":
      result.value = count1 * count2;
      break;
    case "/":
      result.value = parseFloat((count1 / count2).toFixed(2));
      break;
    default:
      result.value = 0;
  }
}
</script>

<template>
  <h1>{{ msg }}</h1>
  <div class="card">
    <q-input
      v-model.number="count1"
      type="number"
      filled
      @change="count(count1, count2, operation)"
    />

    <q-select
      filled
      v-model="operation"
      :options="options"
      label="Filled"
      @update:model-value="count(count1, count2, operation)"
    />

    <q-input
      v-model.number="count2"
      type="number"
      filled
      @change="count(count1, count2, operation)"
    />
    <q-input v-model.number="result" type="number" :readonly="true" />
  </div>
</template>

<style scoped></style>
