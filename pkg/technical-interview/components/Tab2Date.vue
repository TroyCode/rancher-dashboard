<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue';
import { useStore } from 'vuex';
import dayjs from 'dayjs';
import LabeledInput from '@components/Form/LabeledInput/LabeledInput.vue';
import DateFormatter from '@shell/components/formatter/Date.vue';
import { DATE_FORMAT, TIME_FORMAT } from '@shell/store/prefs';
import { escapeHtml } from '@shell/utils/string';

const store = useStore();
const now = ref(dayjs());
let timer: any;

const offsetHours = ref(0);
const offsetMinutes = ref(0);
const offsetSeconds = ref(0);

onMounted(() => {
  timer = setInterval(() => {
    now.value = dayjs();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});

const newDateTime = ref<dayjs.Dayjs | null>(null);

const result = computed(() => {
  if (!newDateTime.value) {
    return '';
  }

  const nowUnix = now.value.unix();
  const newUnix = newDateTime.value.unix();

  if (nowUnix < newUnix) {
    return 'Before';
  } else if (nowUnix > newUnix) {
    return 'After';
  }

  return 'Same';
});

const calculate = () => {
  newDateTime.value = now.value
    .add(Number(offsetHours.value), 'hour')
    .add(Number(offsetMinutes.value), 'minute')
    .add(Number(offsetSeconds.value), 'second');
};

const dateFormat = computed(() => escapeHtml(store.getters['prefs/get'](DATE_FORMAT)));
const timeFormat = computed(() => escapeHtml(store.getters['prefs/get'](TIME_FORMAT)).replace(':ss', ''));

const formattedNow = computed(() => {
  return dayjs(now.value).format(`${ dateFormat.value } ${ timeFormat.value }`);
});
</script>

<template>
  <div>
    <div class="mb-20">
      <h3>
        Current Date and Time: {{ formattedNow }}
      </h3>
    </div>

    <div class="row mb-20">
      <div class="col span-4">
        <LabeledInput
          v-model:value.number="offsetHours"
          type="number"
          label="Offset Hours"
        />
      </div>
      <div class="col span-4">
        <LabeledInput
          v-model:value.number="offsetMinutes"
          type="number"
          label="Offset Minutes"
        />
      </div>
      <div class="col span-4">
        <LabeledInput
          v-model:value.number="offsetSeconds"
          type="number"
          label="Offset Seconds"
        />
      </div>
    </div>

    <div class="mb-20">
      <button
        class="btn role-primary"
        @click="calculate"
      >
        Set
      </button>
    </div>

    <div
      v-if="newDateTime"
      class="mb-20"
    >
      <h3>
        New Date and Time: <DateFormatter :value="newDateTime" />
      </h3>
      <p v-if="result">
        The current date and time is <strong>{{ result }}</strong> the new date and time.
      </p>
    </div>
  </div>
</template>
