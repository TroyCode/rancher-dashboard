<script lang="ts">
import { defineComponent } from 'vue';
import Tab from '@shell/components/Tabbed/Tab.vue';
import Tabbed from '@shell/components/Tabbed/index.vue';

import Tab1Counter from '../components/Tab1Counter.vue';
import Tab2Date from '../components/Tab2Date.vue';
import Tab3Json from '../components/Tab3Json.vue';
import Tab4CoinChange from '../components/Tab4CoinChange.vue';
import Tab5Parentheses from '../components/Tab5Parentheses.vue';

export default defineComponent({
  name: 'TabPage',

  layout: 'plain',

  components: {
    Tab,
    Tabbed,

    Tab1Counter,
    Tab2Date,
    Tab3Json,
    Tab4CoinChange,
    Tab5Parentheses,
  },

  data() {
    return { };
  },

  computed: {},

  methods: {}

});
</script>

<template>
  <div>
    <h1>{{ t('interview.tabPage.title') }}</h1>

    <Tabbed>
      <Tab name="Tab 1: Counter">
        <Tab1Counter />
      </Tab>

      <Tab name="Tab 2: Date">
        <Tab2Date />
      </Tab>

      <Tab name="Tab 3: JSON Swap">
        <Tab3Json />
      </Tab>

      <Tab name="Tab 4: Coin Change">
        <Tab4CoinChange />
      </Tab>

      <Tab name="Tab 5: Parentheses">
        <Tab5Parentheses />
      </Tab>
    </Tabbed>
  </div>
</template>
