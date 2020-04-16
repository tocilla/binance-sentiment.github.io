<template>
  <div id="app">
    <h1>Binance Sentiment</h1>
    <h2>Binance Top and Global Long/Short Ratio (Position)</h2>
    <apexchart
      type="line"
      height="350"
      :options="chartOptions"
      :series="topSeries"
    >
    </apexchart>
    <h2>Binance Global Long/Short Ratio (Position)</h2>
    <apexchart
      type="line"
      height="350"
      :options="chartOptions"
      :series="globalSeries"
    >
    </apexchart>
  </div>
</template>

<script>
import VueApexCharts from "vue-apexcharts";
import axios from "axios";

const BASE_URL = "http://95.215.46.240:3000";
export default {
  name: "app",
  components: {
    apexchart: VueApexCharts
  },
  async mounted() {
    const response = await axios.get(BASE_URL);
    if (response) {
      this.binanceData = response.data;
    }
    console.log(this.binanceData);
  },
  data() {
    return {
      binanceData: [],
      chartOptionsData: {
        chart: {
          height: 350,
          type: "line",
          zoom: {
            enabled: false
          }
        },
        dataLabels: {
          enabled: false
        },
        stroke: {
          curve: "straight"
        },
        title: {
          text: "Product Trends by Month",
          align: "left"
        },
        grid: {
          row: {
            colors: ["#f3f3f3", "transparent"], // takes an array which will be repeated on columns
            opacity: 0.5
          }
        },
        yaxis: {
          title: {
            text: "Ratio"
          }
          // min: 0,
          // max: 2
        },
        tooltip: {
          x: {
            format: "dd/MM/yy HH:mm"
          }
        }
      }
    };
  },
  computed: {
    xaxis() {
      return {
        type: "datetime",
        categories: this.binanceData.map(data => data.timeframe)
      };
    },
    topSeries() {
      return [
        {
          name: "Top Long/Short Position Ratio",
          data: this.binanceData.map(data => data.top_long_short_ratio)
        }
      ];
    },
    globalSeries() {
      return [
        {
          name: "Global Long/Short Position Ratio",
          data: this.binanceData.map(data => data.global_long_short_ratio)
        }
      ];
    },
    chartOptions() {
      return {
        ...this.chartOptionsData,
        xaxis: this.xaxis
      };
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
