<template>
  <section class="column alignCenter">
    <div class="row">
      <weather-data-box :isMain="true" :icon="forecast.icon"></weather-data-box>
      <weather-data-box
        :isMain="true"
        :value="forecast.temp"
        :units="temperature"
      ></weather-data-box>
    </div>
    <div class="row">
      <weather-data-box
        v-for="(item, index) in forecast.details"
        :key="index"
        :label="item.label"
        :units="item.units"
        :value="item.value"
      ></weather-data-box>
    </div>
  </section>
</template>

<script>
import WeatherDataBox from "./WeatherDataBox.vue";
export default {
  components: { WeatherDataBox },
  name: "TodayForecast",
  props: {
    forecast: { type: Object },
  },
  computed: {
    temperature() {
      return `<span>H:${this.forecast.high}</span><span>L:${this.forecast.low}</span>`;
    },
  },
};
</script>

<style scoped>
.row {
  margin-bottom: 55px;
}
</style>
