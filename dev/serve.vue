<script>
import Vue from 'vue';
import VueSingleSelectV2 from '@/vue-single-select-v2.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueSingleSelectV2
  }
});
</script>

<template>
  <div id="app">
    <vue-single-select-v2 
      :value="0"
      :options="['abc', 'def', 'ghi']"
    />
  </div>
</template>
