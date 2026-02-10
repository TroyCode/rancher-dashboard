<script setup lang="ts">
import { ref, computed } from 'vue';
import LabeledInput from '@components/Form/LabeledInput/LabeledInput.vue';

function getMinCoins(coins: number[], amount: number): number[] | null {
  if (amount === 0) return [];

  const sortedCoins = [...coins].sort((a, b) => b - a);
  const result: number[] = [];
  let remaining = amount;

  for (const coin of sortedCoins) {
    while (remaining >= coin) {
      remaining -= coin;
      result.push(coin);
    }
  }

  return remaining === 0 ? result : null;
}

const coinsInput = ref('1, 2, 5');
const amountInput = ref(11);

const result = computed(() => {
  const coins = coinsInput.value
    .split(',')
    .map((s) => parseInt(s.trim()))
    .filter((n) => !isNaN(n));

  if (coins.length === 0) return null;

  return getMinCoins(coins, Number(amountInput.value));
});
</script>

<template>
  <div>
    <div class="row mb-20">
      <div class="col span-6">
        <LabeledInput
          v-model:value="coinsInput"
          label="Coins (comma separated)"
          placeholder="e.g. 1, 2, 5"
        />
      </div>
      <div class="col span-6">
        <LabeledInput
          v-model:value.number="amountInput"
          type="number"
          label="Amount"
        />
      </div>
    </div>

    <div class="mb-20">
      <h3>Result:</h3>
      <div
        v-if="result !== null"
      >
        <span v-if="result.length === 0">[] (Amount is 0)</span>
        <span v-else>[{{ result.join(', ') }}]</span>
      </div>
      <div
        v-else
        class="text-error"
      >
        null
      </div>
    </div>
  </div>
</template>
