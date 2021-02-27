<template>
  <div class="weather-box primary alignLeft" :class="isMain ? 'alignCenter' : 'wideBox'">
    <p v-if="!isMain" class="text-shadow">{{ label }}</p>
    <div
      v-if="value"
      v-html="value"
      class="value text-shadow"
      :class="{ mainValue: isMain }"
    ></div>
    <i v-if="icon" class="mainValue text-shadow" :class="icon"></i>
    <div class="text-shadow units" v-html="units"></div>
  </div>
</template>

<script>
export default {
  name: "WeatherDataBox",
  props: {
    value: {},
    icon: { type: String },
    isMain: { type: Boolean, default: false },
    label: { type: String },
    units: { type: String },
  },
};
</script>

<style scoped>
.weather-box {
  border-radius: 10px;
  color: #fff;
  padding: 10px 15px;
  display: flex;
  max-height: 180px;
  max-width: 160px;
  flex-direction: column;
  justify-content: center;
  box-shadow: 2px 2px #9e5626;
}
.weather-box > * {
  margin: 0;
  margin-bottom: 5px;
}
.text-shadow {
  text-shadow: 1px 2px #010101;
}
.wideBox {
  width: 155px;
  height: 120px;
}
.value {
  font-size: 1.8em;
}
.mainValue {
  font-size: 5em;
}

.units {
  width: 100%;
  justify-content: space-between;
  display: flex;
}
</style>
