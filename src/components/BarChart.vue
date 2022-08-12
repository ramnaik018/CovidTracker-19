<template>
  <!-- <button @click="update()">Update Charts</button> -->
  <h1
    style="
      font-weight: bold;
      text-align: center;
      font-size: 30px;
      margin-top: 30px;
    "
  >
    {{ label }}
  </h1>
  <Bar
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
    style="padding: 40px"
  />
</template>
<script>
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

export default {
  name: "BarChart",
  components: { Bar },
  props: {
    chartId: {
      type: String,
      default: "bar-chart",
    },
    datasetIdKey: {
      type: String,
      default: "label",
    },
    width: {
      type: Number,
      default: 300,
    },
    height: {
      type: Number,
      default: 400,
    },
    cssClasses: {
      default: "",
      type: String,
    },
    styles: {
      type: Object,
      default: () => {},
    },
    plugins: {
      type: Object,
      default: () => {},
    },
    allData: {
      type: Array,
    },
    allDates: {
      type: Array,
    },
    label: {
      type: String,
    },
    backgroundColor: {
      type: String,
    },
  },
  data() {
    return {
      chartData: {
        // labels: ["January", "February", "March", "a", "b", "c", "d"],
        // datasets: [{ data: [40, 20, 12, 4, 5, 6, 7] }],
        labels: this.allDates,
        datasets: [
          {
            data: this.allData,
            label: this.label,
            backgroundColor: this.backgroundColor,
          },
        ],
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
};
</script>
