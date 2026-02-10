<script setup lang="ts">
import { ref, computed } from 'vue';
import LabeledInput from '@components/Form/LabeledInput/LabeledInput.vue';

function isValidParentheses(s: string): boolean {
  const stack: string[] = [];
  const map: Record<string, string> = {
    ')': '(',
    ']': '[',
    '}': '{'
  };

  for (const char of s) {
    if (Object.values(map).includes(char)) {
      stack.push(char);
    } else if (map[char]) {
      if (stack.pop() !== map[char]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}

const input = ref('()');

const valid = computed(() => isValidParentheses(input.value));
</script>

<template>
  <div>
    <div class="mb-20">
      <LabeledInput
        v-model:value="input"
        label="Input String"
        placeholder="e.g. {}[]()"
      />
    </div>

    <div class="mb-20">
      <h3>Result:</h3>
      <div :class="valid ? 'text-success' : 'text-error'">
        {{ valid ? 'true' : 'false' }}
      </div>
    </div>
  </div>
</template>
