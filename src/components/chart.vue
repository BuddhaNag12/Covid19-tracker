
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
              "rgba(0, 0, 255, 0.8)",
              "rgba(209, 107, 165, 0.8)",
              "rgba(0, 255, 0, 0.6)",
              "rgba(255, 0, 0, 0.8)",
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