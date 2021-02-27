<template>
  <div class="day-item" @click="$emit('onForecast')">
    <i v-if="forecast.icon" class="forecast-icon" :class="forecast.icon"></i>
    <div class="text-left">
      <p class="date-label">{{ forecast.date }}</p>
      <p class="temperature" v-html="temperature"></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ForecastDayItem",
  props: {
    forecast: { type: Object },
  },
  computed: {
    temperature() {
      return `<span>H:${this.forecast.high}</span> <span>L:${this.forecast.low}</span>`;
    },
  },
};
</script>

<style scoped>
.day-item {
  margin-bottom: 10px;
  display: flex;
  cursor: pointer;
}
.text-left {
  text-align: left;
}
p {
  margin: 0;
}

.forecast-icon {
  font-size: 1.3em;
  margin-right: 15px;
  width: 30px;
}
.temperature {
  color: #e29c47;
}
</style>
