<template>
  <div>
    <table class="content">
      <thead>
        <tr>
          <th>Date</th>
          <th>Temperature C</th>
          <th>Summary</th>
        </tr>
      </thead>
      <tbody>
        <tr :key="index" v-for="(f, index) in store.forecasts">
          <td>{{ f.date }}</td>
          <td>{{ f.temperatureC }}</td>
          <td>{{ f.summary }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import store from '@/store';
export default Vue.extend({
  data() {
    return {
      store,
    };
  },
  async created() {
    await store.loadWeatherForecasts();
  }
});
</script>
<style>
table.content {
  margin-right: auto;
  margin-left: auto;
}
</style>