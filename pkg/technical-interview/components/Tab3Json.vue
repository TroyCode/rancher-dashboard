<script setup lang="ts">
import { ref } from 'vue';
import FileSelector from '@shell/components/form/FileSelector.vue';
import Banner from '@components/Banner/Banner.vue';

const fileContent = ref<string | null>(null);
const result = ref<any>(null);
const error = ref<string | null>(null);

const onFileSelected = (content: string) => {
  error.value = null;
  fileContent.value = content;
  result.value = null;
  try {
    JSON.parse(content);
  } catch (e) {
    error.value = 'Invalid JSON file';
  }
};

const swapJson = () => {
  error.value = null;
  if (!fileContent.value) return;

  try {
    const obj = JSON.parse(fileContent.value);

    result.value = performSwap(obj);
  } catch (e) {
    error.value = `Error processing JSON: ${ (e as Error).message }`;
  }
};

const performSwap = (obj: any): any => {
  if (Array.isArray(obj)) {
    return obj.map((item) => performSwap(item));
  } else if (obj !== null && typeof obj === 'object') {
    const newObj: any = {};

    for (const key in obj) {
      const value = obj[key];

      if (['string', 'number', 'boolean'].includes(typeof value)) {
        // Swap primitive values and keys
        newObj[String(value)] = key;
      } else {
        // Keep Object the same
        newObj[key] = value;
      }
    }

    return newObj;
  }

  return obj;
};
</script>

<template>
  <div>
    <div class="mb-20">
      <FileSelector
        label="Upload JSON File"
        accept=".json"
        @selected="onFileSelected"
        @error="error = $event"
      />
    </div>

    <Banner
      v-if="error"
      color="error"
      :label="error"
    />

    <div
      v-if="fileContent"
      class="row mb-20"
    >
      <div class="col span-6">
        <h3>Original Content:</h3>
        <pre>{{ fileContent }}</pre>
      </div>
      <div class="col span-6">
        <h3>Swapped Content:</h3>
        <pre
          v-if="result"
        >{{ JSON.stringify(result, null, 2) }}</pre>
        <div
          v-else
          class="text-muted"
        >
          Click "Swap" to see result
        </div>
      </div>
    </div>

    <div
      v-if="fileContent && !error"
      class="mb-20"
    >
      <button
        class="btn role-primary"
        @click="swapJson"
      >
        Swap Key/Value
      </button>
    </div>
  </div>
</template>
