
<script>
import { Bar } from 'vue-chartjs'

export default {
  extends: Bar,

  props:['confirmed','recovered','deaths','active','country'],
  data(){
    return{
      options: {
      type: Object,
      default: null
    },
      chartdata: {
        labels: ["Confirmed","Active", "Recovered", "Deaths"],
        datasets: [
          {
            label:this.country,
            backgroundColor: [
              "rgba(0, 0, 255, 0.5)",
              "rgba(0, 255, 244, 0.5)",
              "rgba(0, 255, 0, 0.5)",
              "rgba(255, 0, 0, 0.5)",
            ],
            data: [this.confirmed,this.active,this.recovered,this.deaths]
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