<template>
  <div id="app">
    <div class="row full-height">
      <div class="col-7 center-items">
        <today-forecast
          v-if="forecast.length > 0 && currentForecast"
          :forecast="currentForecast"
          class="pt-50"
        ></today-forecast>
      </div>
      <div class="col-3 side-color column alignLeft pd-20">
        <h2>Forecast</h2>
        <div class="column">
          <forecast-day-item
            v-for="(item, index) in futureForecast"
            :key="index"
            :forecast="item"
            @onForecast="onChangeForecast(index)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodayForecast from "./components/TodayForecast.vue";
import { getWeatherForecast } from "./js/weatherService";
import { weatherProps, WEATHER_STATES_TO_ICON } from "./js/lib";
import ForecastDayItem from "./components/ForecastDayItem.vue";
import moment from "moment";

export default {
  name: "App",
  data() {
    return {
      currentForecast: {},
      forecast: [],
      futureForecast: [],
      error: null,
    };
  },
  components: {
    TodayForecast,
    ForecastDayItem,
  },
  methods: {
    async fetchForecast() {
      try {
        this.forecast = await getWeatherForecast();
        this.futureForecast = this.forecast.reduce((res, item) => {
          return [...res, this.setCurrentForecast(item)];
        }, []);
        this.currentForecast = this.futureForecast[0];
        this.futureForecast.shift();
      } catch (err) {
        this.error = err;
      }
    },
    onChangeForecast(index) {
      this.currentForecast = this.futureForecast[index];
    },
    getForecastDetails(data) {
      return weatherProps.map((item) => {
        const newItem = { ...item, value: data[item.propName] };
        if (item.propName == "wind_speed") {
          newItem.value = Math.ceil(newItem.value);
        }
        return newItem;
      });
    },
    setCurrentForecast(data) {
      const {
        weather_state_abbr: icon,
        the_temp: temp,
        min_temp,
        max_temp,
      } = data;

      const date = moment(data.applicable_date).format("MMM Do YY");

      return {
        icon: WEATHER_STATES_TO_ICON[icon],
        temp: Math.round(temp),
        low: Math.round(min_temp),
        high: Math.round(max_temp),
        date,
        details: this.getForecastDetails(data),
      };
    },
  },
  created() {
    this.fetchForecast();
  },
};
</script>


<style>
* {
  box-sizing: border-box;
}
html,
body {
  padding: 0;
  margin: 0;
  font-size: 1em;
  color: #fff;
}
.side-color {
  background-color: rgba(154, 80, 35, 0.6);
}
.primary {
  background-color: #b05f2a;
}
#app {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: rgb(221, 114, 50);
  background: linear-gradient(
    rgba(221, 114, 50, 1) 0%,
    rgba(211, 157, 76, 1) 100%
  );
}
.full-height {
  height: 100vh;
}
.center-items {
  justify-content: center;
}
.pt-50 {
  padding-top: 50px;
}
.pd-20 {
  padding: 20px;
}

/********************** */
.alignCenter {
  align-items: center !important;
}
.alignLeft {
  align-items: baseline;
}

.row,
.column {
  display: flex;
}
.column {
  flex-direction: column;
}
.row > div {
  display: flex;
  flex: 1;
}
.row > div:not(:last-child) {
  margin: 0 10px 0 0;
}

.col-1 {
  max-width: calc(100% / 12);
}
.col-2 {
  max-width: calc(100% / 12 * 2);
}
.col-3 {
  max-width: calc(100% / 12 * 3);
}
.col-4 {
  max-width: calc(100% / 12 * 4);
}
.col-5 {
  max-width: calc(100% / 12 * 5);
}
.col-6 {
  max-width: calc(100% / 2);
}
</style>
