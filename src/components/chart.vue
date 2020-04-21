
<script>
import { Bar } from 'vue-chartjs'

export default {
  extends: Bar,

  props:['confirmed','recovered','deaths'],
  data(){
    return{
      options: {
      type: Object,
      default: null
    },
      chartdata: {
        labels: ["Confirmed", "Recovered", "Deaths"],
        datasets: [
          {
            label: "People",
            backgroundColor: [
              "rgba(0, 0, 255, 0.5)",
              "rgba(0, 255, 0, 0.5)",
              "rgba(255, 0, 0, 0.5)"
            ],
            data: [this.confirmed,this.recovered,this.deaths]
          }
        ]
       },
    }
  },
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>