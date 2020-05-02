<script>
import { Line } from "vue-chartjs";

export default {
  extends: Line,
  props: {
    dates:Array,
     dailyConfirmed:Array,
     dailyDeaths:Array,
    },
  data() {
    return {
      chartdata: {
        labels: this.dates,
        datasets: [
          {
            label: "Infected",
            borderColor: "#3333ff",
             backgroundColor: "rgba(0, 0, 255, 0.2)",
            fill: true,
            data: this.dailyConfirmed
          },
          {
            label: "Deaths",
            borderColor: "red",
            backgroundColor: "rgba(255, 0, 0, 0.5)",
            fill: true,
            data: this.dailyDeaths
          }
        ]
      }
    };
  },

  mounted() {
    this.renderChart(this.chartdata, this.options);
  },


};
</script>