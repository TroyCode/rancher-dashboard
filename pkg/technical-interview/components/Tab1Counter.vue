<script setup lang="ts">
import { ref } from 'vue';

const count = ref(42);

const decrement = () => {
  count.value--;
};
</script>

<template>
  <div>
    <div class="mb-20">
      Counter: {{ count }}
    </div>
    <button
      class="btn role-primary"
      @click="decrement"
    >
      Decrement
    </button>
  </div>
</template>
